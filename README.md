# ESP32-Cam-APSTA

Eigentlich geht es nur darum den ESP32-Cam-Server sowohl über einen möglicherweise vorhandenen Router als auch direkt über das Device selbst zu erreichen. Die klassische Konfiguration sieht leider nur ein entweder STA oder AP Mode vor.
Zum Glück ist aber der Umbau relativ simpel....

Als erstes müssen zusätzlich zu den Credentials des AP im Station Mode noch die Credentials des SoftAP erzeugt werden:

```
const char* ssid = "glibber";
const char* password = "12345678";
const char* soft_ap_ssid = "ESP32Cam";
const char* soft_ap_password = "12345678";
```
Schön darauf achten das das Passwort, wenn es nicht leer ist, mindestens 8 Zeichen lang ist!

Jetzt muss nur noch der Code umgebaut werden um sowohl im Sta als auch im AP Mode gleichzeitig zu arbeiten:

```
  WiFi.mode(WIFI_MODE_APSTA);
 
  WiFi.softAP(soft_ap_ssid, soft_ap_password);
  WiFi.begin(ssid, password);

startCameraServer();

  Serial.print("Camera Ready! Use 'http://");
  Serial.print(WiFi.softAPIP());
  Serial.println("' to connect over SoftAP or wait for connection to local AP");
 
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  
  Serial.println("");
  Serial.println("WiFi AP connected");

//  startCameraServer();

  Serial.print("Use 'http://");
  Serial.print(WiFi.localIP());
  Serial.println("' to connect over local AP");
}
```

Es ist wie immer, entweder selber anpassen oder das fertige .ino downloaden.
