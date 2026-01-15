

#include <WiFi.h>
#include <WebServer.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include "DHT.h"
#include "webpage.h"
#include "LittleFS.h"
// --- KONFIGURATION ---
const char* ssid = "Wetterstation von John";
const char* password = "nwtprojekt";

// OLED Einstellungen
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

// Sensor Einstellungen
#define DHTPIN 4
#define DHTTYPE DHT11
#define SOLARPIN 34

DHT dht(DHTPIN, DHTTYPE);
WebServer server(80);

// Globale Variablen für Messwerte
float t = 0.0;
float h = 0.0;
float solarV = 0.0;
// ZWEI Zeit-Variablen 
unsigned long lastMeasureTime = 0; // Für Live-Werte (2 Sek)
unsigned long lastLogTime = 0;     // Für CSV-Speichern (z.B. 5 Min)

File uploadFile; // Hilfsvariable für den Dateiupload

void setup() {
  Serial.begin(115200);

  // 1. Hardware Starten
  dht.begin();
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("OLED Fehler"));
    for(;;);
  }
  display.clearDisplay();
  display.setTextColor(SSD1306_WHITE);
  
  // 2. Access Point starten (Der "Hotspot")
  display.setCursor(0,0);
  display.println("Starte Hotspot...");
  display.display();
  
  // WICHTIG: Hier nutzen wir softAP statt begin
  WiFi.softAP(ssid, password);
  IPAddress IP = WiFi.softAPIP();

  // Dateisystem starten
  if(!LittleFS.begin(true)){
    Serial.println("LittleFS Fehler!");
    return;
  }

  // 3. Webserver Routen definieren
  
  // Hauptseite
  server.on("/", []() {
    server.send(200, "text/html", index_html);
  });

  // Daten-API
  server.on("/data", []() {
    String json = "{";
    json += "\"temperature\":" + String(t) + ",";
    json += "\"humidity\":" + String(h) + ",";
    json += "\"solar\":" + String(solarV);
    json += "}";
    server.send(200, "application/json", json);
  });

  // CSV Download
  server.on("/download", []() {
    File file = LittleFS.open("/data.csv", "r");
    if (file) {
        server.streamFile(file, "text/csv");
        file.close();
    } else {
        server.send(404, "text/plain", "Keine Daten");
    }
  });

  // CSV Löschen
  server.on("/clear", []() {
    LittleFS.remove("/data.csv");
    server.send(200, "text/plain", "Daten gelöscht");
  });

  // --- WARTUNGS-ROUTEN (Upload Tool) ---
  
  // Upload Formular
  server.on("/upload", HTTP_GET, []() {
    String html = "<body><h1>Datei Upload</h1>";
    html += "<form method='POST' action='/upload' enctype='multipart/form-data'>";
    html += "<input type='file' name='file'><br><br>";
    html += "<input type='submit' value='Hochladen'>";
    html += "</form></body>";
    server.send(200, "text/html", html);
  });

  // Upload Verarbeitung
  server.on("/upload", HTTP_POST, []() {
    server.send(200, "text/plain", "Upload erfolgreich! Starte neu...");
    delay(1000);
    ESP.restart(); 
  }, []() {
    HTTPUpload& upload = server.upload();
    if (upload.status == UPLOAD_FILE_START) {
      String filename = upload.filename;
      if (!filename.startsWith("/")) filename = "/" + filename;
      Serial.print("Upload start: "); Serial.println(filename);
      uploadFile = LittleFS.open(filename, "w");
    } 
    else if (upload.status == UPLOAD_FILE_WRITE) {
      if (uploadFile) uploadFile.write(upload.buf, upload.currentSize);
    } 
    else if (upload.status == UPLOAD_FILE_END) {
      if (uploadFile) {
        uploadFile.close();
        Serial.print("Upload beendet: "); Serial.println(upload.totalSize);
      }
    }
  });

  // Chart.js lokal ausliefern
  server.on("/chart.js", []() {
    File file = LittleFS.open("/chart.js", "r");
    if (file) {
      server.streamFile(file, "application/javascript");
      file.close();
    } else {
      server.send(404, "text/plain", "Chart.js nicht gefunden!");
    }
  });

  // 4. JETZT erst den Server starten
  server.begin();

  // 5. IP Adresse auf OLED anzeigen
  display.clearDisplay();
  display.setCursor(0,0);
  display.println("WLAN AP Aktiv!");
  display.println("Name:");
  display.println(ssid); 
  display.println("IP:");
  display.println(IP);   
  display.display();
  
  Serial.print("AP IP Adresse: ");
  Serial.println(IP);
}


void loop() {
  server.handleClient(); // Webserver Anfragen bearbeiten
  
  IPAddress IP = WiFi.softAPIP();
  
  unsigned long currentMillis = millis();

  // --- TEIL A: LIVE MESSUNG (Alle 2 Sekunden) ---
  if (currentMillis - lastMeasureTime > 2000) {
    lastMeasureTime = currentMillis;
    
    float newH = dht.readHumidity();
    float newT = dht.readTemperature();
    int solarRaw = analogRead(SOLARPIN);
    
    // Fehlercheck: Nur aktualisieren, wenn Sensor OK
    if (!isnan(newH) && !isnan(newT)) {
      h = newH;
      t = newT;
      solarV = (solarRaw / 4095.0) * 3.3 * 2; // Spannungsteiler Faktor 2
    
    }

    // OLED Update (Live-Status)
    display.clearDisplay();
    display.setCursor(0, 0);
    display.println(IP); 
    display.drawLine(0, 10, 128, 10, SSD1306_WHITE);
    
    display.setCursor(0, 20);
    display.print("Temp: "); display.print(t, 1); display.println(" C");
    display.print("Feucht: "); display.print(h, 0); display.println(" %");
    display.print("Solar: "); display.print(solarV, 2); display.println(" V");
    display.display();
  
  Serial.print("Live Update -> T: "); Serial.println(t);

  
  }

  // --- TEIL B: LANGZEIT LOGGING (Alle 5 Minuten = 300000 ms) ---
  // Zum Testen kannst du hier 10000 (10 Sek) nehmen, später 300000
  if (currentMillis - lastLogTime > 300000) { 
    lastLogTime = currentMillis;

    // In Datei schreiben (Append Mode "a")
    File file = LittleFS.open("/data.csv", "a");
    if(file){
      // Format: Zeit,Temp,Feuchte,Solar (Alles in EINER Zeile!)
      file.print(currentMillis/1000); 
      file.print(",");
      file.print(t); 
      file.print(",");
      file.print(h); // Hier war vorher println -> falsch
      file.print(",");
      file.println(solarV); // Erst am Ende eine neue Zeile
      file.close();
      Serial.println("Daten in CSV gespeichert.");
    } else {
      Serial.println("Fehler beim Öffnen der CSV!");
    }
  }
}