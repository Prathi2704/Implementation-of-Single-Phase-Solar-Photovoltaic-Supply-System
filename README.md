# Implementation-of-Single-Phase-Solar-Photovoltaic-Supply-System
Implementation of Single Phase Solar Photovoltaic Supply System
# Import the required libraries
import minimalmodbus
import time

# Define the Modbus communication settings
inverter_port = '/dev/ttyUSB0'  # Replace with the correct serial port
inverter_address = 1  # Replace with the correct Modbus address

# Create a Modbus instrument for communication with the inverter
inverter = minimalmodbus.Instrument(inverter_port, inverter_address)
inverter.serial.baudrate = 9600
inverter.serial.bytesize = 8
inverter.serial.parity = 'N'
inverter.serial.stopbits = 1

# Function to read data from the inverter
def read_inverter_data():
    try:
        voltage = inverter.read_float(0x3100)
        current = inverter.read_float(0x3102)
        power = inverter.read_float(0x3106)
        # You can add more parameters as needed

        return voltage, current, power
    except Exception as e:
        print(f"Error reading data from the inverter: {e}")
        return None

# Main loop for data monitoring
while True:
    data = read_inverter_data()
    if data:
        voltage, current, power = data
        print(f"Voltage: {voltage} V, Current: {current} A, Power: {power} W")
        # You can add data logging or further processing here

    time.sleep(5)  # Read data every 5 seconds (adjust as needed)
