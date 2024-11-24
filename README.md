import network
import espnow
from machine import Pin

# A WLAN interface must be active to send()/recv()
sta = network.WLAN(network.STA_IF)
sta.active(True)
sta.disconnect()  # For ESP8266

# Initialize ESP-NOW
esp = espnow.ESPNow()
esp.active(True)

# Create a pin to control an LED (GPIO 2 is common for ESP32)
led_pin = Pin(2, Pin.OUT)

# Function to handle incoming messages
def on_receive_message():
    while True:
        peer, msg = esp.recv()  # Wait for a message
        
        if msg:
            # Decode the message
            message = msg.decode('utf-8')
            print(f"Received message: {message}")
            
            # Take action based on the received message
            if message == "ledOn":
                led_pin.on()  # Turn the LED on
                print("LED turned ON")
            elif message == "ledOff":
                led_pin.off()  # Turn the LED off
                print("LED turned OFF")
            else:
                print("Unknown command")

# Call the function to start listening for messages
on_receive_message()
