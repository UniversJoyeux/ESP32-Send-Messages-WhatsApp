# ESP32-Send-Messages-WhatsApp
This example demonstrates how to send messages to WhatsApp using ESP32 along with Arduino IDE. For this to work we should install WhatsApp and Arduino IDE, and acquire ESP32 microcontroller.

## Step 1: for Getting the CallMeBot API KEY
To send messages to your WhatsApp account with the ESP32, we’ll use a free API service called CallMeBot service.

* Go to Sketch > Include Library > Manage Libraries and search for URLEncode library by Masayuki Sugahara and install it as shown below.
* Click this link [CallMeBot](https://www.callmebot.com/blog/free-api-whatsapp-messages/)
### Instructions:
1. Add the phone number **beginning with plus(+) sign** into your **Phone Contacts** and name it it as you wish.
2. Send this message **"I allow callmebot to send me messages"** to the new Contact created using WhatsApp.
3. Wait until you receive the message **"API Activated for your phone number. Your APIKEY is xxxxx"** from the bot.
Note: If you don't receive the ApiKey in 2 minutes, please try again after 24hs.
4. The WhatsApp message from the bot will contain the _**APIKEY**_ needed to send messages using the API.
### Step 2: ESP32 Code (Copy and paste on Arduino IDE)
```
  /* 
  Rui Santos
  Complete project details at https://RandomNerdTutorials.com/esp32-send-messages-whatsapp/
  
  Permission is hereby granted, free of charge, to any person obtaining a copy
  of this software and associated documentation files.
  
  The above copyright notice and this permission notice shall be included in all
  copies or substantial portions of the Software.
*/

#include <WiFi.h>    
#include <HTTPClient.h>
#include <UrlEncode.h>

// your network credentials
const char* ssid = "REPLACE_WITH_YOUR_SSID";
const char* password = "REPLACE_WITH_YOUR_PASSWORD";

// +international_country_code + phone number
// United States +1, example: +151912345678
String phoneNumber = "REPLACE_WITH_YOUR_PHONE_NUMBER";
String apiKey = "REPLACE_WITH_API_KEY";

void sendMessage(String message){

  // Data to send with HTTP POST
  String url = "https://api.callmebot.com/whatsapp.php?phone=" + phoneNumber + "&apikey=" + apiKey + "&text=" + urlEncode(message);    
  HTTPClient http;
  http.begin(url);

  // Specify content-type header
  http.addHeader("Content-Type", "application/x-www-form-urlencoded");
  
  // Send HTTP POST request
  int httpResponseCode = http.POST(url);
  if (httpResponseCode == 200){
    Serial.print("Message sent successfully");
  }
  else{
    Serial.println("Error sending the message");
    Serial.print("HTTP response code: ");
    Serial.println(httpResponseCode);
  }

  // Free resources
  http.end();
}

void setup() {
  Serial.begin(115200);

  WiFi.begin(ssid, password);
  Serial.println("Connecting");
  while(WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.print("Connected to WiFi network with IP Address: ");
  Serial.println(WiFi.localIP());

  // Send Message to WhatsAPP
  sendMessage("Hello from ESP32!");
}
```
## Step 3: Information to Make It Works
### 1. Enter your network credentials by replacing the words between the quotes:
* const char* ssid = "REPLACE_WITH_YOUR_SSID";
* const char* password = "REPLACE_WITH_YOUR_PASSWORD";
### 2. Enter your phone number and API key
* String phoneNumber = "REPLACE_WITH_YOUR_PHONE_NUMBER";
* String apiKey = "REPLACE_WITH_YOUR_API_KEY";
### 3. Write the message your to send by replacing the one into the quotes. Ensure the quotes should contain your written message.
* // Send Message to WhatsAPP
  sendMessage("Hello from ESP32!");
## How to Use Example

* How to install the Arduino IDE: [Install Arduino IDE](https://github.com/espressif/arduino-esp32/tree/master/docs/arduino-ide).
* 

#### Using Arduino IDE

* Before Compile/Verify, select the correct board: `Tools -> Board`.
* Select the COM port: `Tools -> Port: xxx` where the `xxx` is the detected COM port.

#### Using Platform IO

* Select the COM port: `Devices` or setting the `upload_port` option on the `platformio.ini` file.

## Example/Log Output

```
Setup done
Scan start
Scan done
17 networks found
Nr | SSID            | RSSI | CH | Encryption
 1 | IoTNetwork      |  -62 |  1 | WPA2
 2 | WiFiSSID        |  -62 |  1 | WPA2-EAP
 3 | B3A7992         |  -63 |  6 | WPA+WPA2
 4 | WiFi            |  -63 |  6 | WPA3
 5 | IoTNetwork2     |  -64 | 11 | WPA2+WPA3
...
```

## Troubleshooting

***Important: Be sure you're using a good quality USB cable and you have enough power source for your project.***

* **Programming Fail:** If the programming/flash procedure fails, try to reduce the serial connection speed.
* **COM port not detected:** Check the USB cable connection and the USB to Serial driver installation.

If the error persist, you can ask help at the official [ESP32 forum](https://esp32.com) or see [Contribute](#contribute).

## Resources

* Arduino-ESP32 Official Repository: [espressif/arduino-esp32](https://github.com/espressif/arduino-esp32)
* CallMeBot: [Free-API](https://www.callmebot.com/blog/free-api-whatsapp-messages/)
* RandomNerdtutorials: [ESP-WhatsApp-Message](https://randomnerdtutorials.com/esp32-send-messages-whatsapp/)
