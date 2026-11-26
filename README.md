# Temperature-and-Humidity-Monitoring-System
To develop an Android application to monitor temperature and humidity values and display warning messages based on predefined limits.

# TEMPERATURE AND HUMIDITY MONITORING SYSTEM

## 1. AIM
To develop an Android application for monitoring temperature and humidity levels, displaying the measured values, and generating alerts when the readings exceed predefined threshold limits.

## 2. SOFTWARE REQUIREMENTS
- Android Studio
- Java Programming Language
- XML for UI Design
- Android SDK
- Android Emulator or Android Smartphone

**Hardware Requirements (Optional):**
- Raspberry Pi
- DHT11 or DHT22 Temperature and Humidity Sensor
- Wi-Fi connectivity
- Jumper wires and breadboard

## 3. THEORY
A Temperature and Humidity Monitoring System is designed to measure and monitor the surrounding environmental conditions.

Temperature indicates the degree of hotness or coldness of the environment, while humidity represents the amount of water vapour present in the air.

The Android application provides a user-friendly interface to enter or receive temperature and humidity readings and display them on the mobile screen.

The application compares the readings with predefined threshold values and displays warning messages whenever the measured values exceed the specified limits.

A DHT11 or DHT22 sensor can be interfaced with a Raspberry Pi to obtain real-time temperature and humidity readings. The Raspberry Pi can transmit the sensor data to the Android application through Wi-Fi communication.

## 4. PROCEDURE
1. Open Android Studio and create a new project.
2. Select an Empty Views Activity and choose Java as the programming language.
3. Provide the application name as **Temperature and Humidity Monitoring System**.
4. Design the user interface using XML.
5. Add input fields to enter temperature and humidity values.
6. Add a Monitor button to process the entered readings.
7. Add TextViews to display the temperature, humidity and monitoring status.
8. Write Java code to retrieve the entered values.
9. Compare the readings with predefined threshold values.
10. Display suitable warning messages if the readings exceed the limits.
11. Run the application using an Android emulator or smartphone.
12. Verify the application by entering different temperature and humidity values.

## 5. ALGORITHM
1. Start the application.
2. Initialize the temperature and humidity input fields.
3. Enter the temperature and humidity readings.
4. Click the Monitor button.
5. Read the entered values.
6. Check whether the temperature exceeds the predefined limit (35°C).
7. Check whether the humidity exceeds the predefined limit (70%).
8. If either value exceeds its limit, display a warning message.
9. Otherwise, display a normal environmental condition message.
10. Display the readings on the application screen.
11. Stop.
# PROGRAM
XML
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/etTemperature"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="Enter Temperature (°C)"
        android:inputType="numberSigned|numberDecimal"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="32dp" />

    <EditText
        android:id="@+id/etHumidity"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="Enter Humidity (%)"
        android:inputType="numberSigned|numberDecimal"
        app:layout_constraintTop_toBottomOf="@+id/etTemperature"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="16dp" />

    <Button
        android:id="@+id/btnCheck"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Check Readings"
        app:layout_constraintTop_toBottomOf="@+id/etHumidity"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="24dp" />

    <TextView
        android:id="@+id/tvTemperature"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Temperature: --"
        android:textSize="18sp"
        app:layout_constraintTop_toBottomOf="@+id/btnCheck"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="32dp" />

    <TextView
        android:id="@+id/tvHumidity"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Humidity: --"
        android:textSize="18sp"
        app:layout_constraintTop_toBottomOf="@+id/tvTemperature"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="16dp" />

    <TextView
        android:id="@+id/tvStatus"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="System Status: NORMAL"
        android:textSize="18sp"
        android:textStyle="bold"
        app:layout_constraintTop_toBottomOf="@+id/tvHumidity"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="24dp" />

</androidx.constraintlayout.widget.ConstraintLayout>
KT
package com.example.temperatureandhumiditymonitoringwithgraphicaldisplayofsensorreadings

import android.app.Activity
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.TextView
import android.widget.Toast

class MainActivity : Activity() {
    private lateinit var etTemperature: EditText
    private lateinit var etHumidity: EditText
    private lateinit var btnCheck: Button
    private lateinit var tvTemperature: TextView
    private lateinit var tvHumidity: TextView
    private lateinit var tvStatus: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        etTemperature = findViewById(R.id.etTemperature)
        etHumidity = findViewById(R.id.etHumidity)
        btnCheck = findViewById(R.id.btnCheck)
        tvTemperature = findViewById(R.id.tvTemperature)
        tvHumidity = findViewById(R.id.tvHumidity)
        tvStatus = findViewById(R.id.tvStatus)

        btnCheck.setOnClickListener { checkReadings() }
    }

    private fun checkReadings() {
        val tempInput = etTemperature.text.toString().trim()
        val humidityInput = etHumidity.text.toString().trim()

        if (tempInput.isEmpty() || humidityInput.isEmpty()) {
            Toast.makeText(
                this,
                "Please enter both readings",
                Toast.LENGTH_SHORT
            ).show()
            return
        }

        try {
            val temperature = tempInput.toDouble()
            val humidity = humidityInput.toDouble()

            if (temperature < -50 || temperature > 100 || humidity < 0 || humidity > 100) {
                Toast.makeText(
                    this,
                    "Enter valid sensor readings",
                    Toast.LENGTH_SHORT
                ).show()
                return
            }

            tvTemperature.text = "Temperature: $temperature °C"
            tvHumidity.text = "Humidity: $humidity %"

            if (temperature > 35 || humidity > 70) {
                tvStatus.text = "WARNING: High Temperature or Humidity!"
            } else {
                tvStatus.text = "System Status: NORMAL"
            }
        } catch (_: NumberFormatException) {
            Toast.makeText(
                this,
                "Invalid input",
                Toast.LENGTH_SHORT
            ).show()
        }
    }
}


## 6. EXPECTED OUTPUT
<img width="1862" height="1012" alt="Screenshot 2026-11-26 142035" src="https://github.com/user-attachments/assets/b485d513-ce42-45f5-ab9c-bacfcda2b285" />


## 7. APPLICATION FEATURES
- User-friendly Android interface.
- Displays temperature and humidity readings.
- Compares readings with predefined threshold values.
- Generates warning messages for abnormal conditions.
- Supports manual entry of sensor readings.
- Can be extended for real-time monitoring using Raspberry Pi and DHT11/DHT22 sensors.
- Can be enhanced with graphical representation and periodic data logging.

## 8. RESULT
The Temperature and Humidity Monitoring System Android application was developed successfully. The application accepts temperature and humidity readings, displays the values and generates alerts when the readings exceed the predefined threshold limits.

## 9. CONCLUSION
The developed application provides a simple method for monitoring environmental temperature and humidity conditions. It demonstrates Android application development using Java and XML. The system can be further enhanced by interfacing a DHT11/DHT22 sensor with Raspberry Pi to enable real-time environmental monitoring through wireless communication.
