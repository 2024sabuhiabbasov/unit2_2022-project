# Unit 2: A Distributed Weather Station for ISAK

## Criteria A: Planning

## Problem definition

Members of R3-14 house at UWC ISAK Japan in Karuizawa have rescued an injured bird named ‘Peter’. Peter was found on the grass in front of the house unable to fly and can barely walk. The house members think that it is dangerous for Peter to be in the wild if he is still injured, so they are taking responsibility in taking care of Peter in a cardboard box and feeding him nuts. As Peter is recovering, he needs a specific condition of temperature and humidity to live in. Members of the house want a program to provide them with the accurate temperature and humidity in the room to be able to correctly control the condition for Peter to live in. 

## Proposed Solution
Considering the client requirements an adequate solution includes a low cost sensing device for humidity and temperature and a custom data script that process and anaysis the samples acquired. For a low cost sensing device an adequate alternative is the DHT11 sensor[^1] which is offered online for less than 5 USD and provides adequare precision and range for the client requirements (Temperature Range: 0°C to 50°C, Humidity Range: 20% to 90%). Similar devices such as the DHT22, AHT20 or the AM2301B [^2] have higher specifications, however the DHT11 uses a simple serial communication (SPI) rather than more eleborated protocols such as the I2C used by the alternatives. For the range, precision and accuracy required in this applicaiton the DHT11 provides the best compromise. Connecting the DHT11 sensor to a computer requires a device that provides a Serial Port communication. A cheap and often used alternative for prototyping is the Arduino UNO microcontroller [^3]. "Arduino is an open-source electronics platform based on easy-to-use hardware and software"[^4]. In additon to the low cost of the Arduino (< 6USD), this devide is programable and expandable[^1]. Other alternatives include diffeerent versions of the original Arduino but their size and price make them a less adequate solution.

Considering the budgetary constrains of the client and the hardware requirements, the software tool that I proposed for this solution is Python. Python is open source, it is mature and supported in mutiple platforms (platform-independent) including macOS, Windows, Linux and can also be used to program the Arduino microprocessor [^5][^6]. In comparison to the alternative C or C++, which share similar features, Python is a High level programming language (HLL) with high abstraction [^7]. For example, memory management is automatic in Python whereas it is responsability of the C/C++ developer to allocate and free up memory [^7], this could result in faster applications but also memory problems. In addition a HLL language will allow me and future developers extend the solution or solve issues proptly.  

**Design statement**

[^1]: Industries, Adafruit. “DHT11 Basic Temperature-Humidity Sensor + Extras.” Adafruit Industries Blog RSS, https://www.adafruit.com/product/386. 
[^2]: Nelson, Carter. “Modern Replacements for DHT11 and dht22 Sensors.” Adafruit Learning System, https://learn.adafruit.com/modern-replacements-for-dht11-dht22-sensors/what-are-better-alternatives.   
[^3]:“How to Connect dht11 Sensor with Arduino Uno.” Arduino Project Hub, https://create.arduino.cc/projecthub/pibots555/how-to-connect-dht11-sensor-with-arduino-uno-f4d239.  
[^4]:Team, The Arduino. “What Is Arduino?: Arduino Documentation.” Arduino Documentation | Arduino Documentation, https://docs.arduino.cc/learn/starting-guide/whats-arduino.  
[^5]:Tino. “Tino/PyFirmata: Python Interface for the Firmata (Http://Firmata.org/) Protocol. It Is Compliant with Firmata 2.1. Any Help with Updating to 2.2 Is Welcome. the Capability Query Is Implemented, but the Pin State Query Feature Not Yet.” GitHub, https://github.com/tino/pyFirmata. 
[^6]:Python Geeks. “Advantages of Python: Disadvantages of Python.” Python Geeks, 26 June 2021, https://pythongeeks.org/advantages-disadvantages-of-python/. 
[^7]: Real Python. “Python vs C++: Selecting the Right Tool for the Job.” Real Python, Real Python, 19 June 2021, https://realpython.com/python-vs-cpp/#memory-management. 
## Success Criteria

1. The solution provides a visual representation of the Humidity and Temperature values inside a dormitory (Local) and outside the house (Remote) for a period of minimum 48 hours. 
1. ```[HL]``` The local variables will be measure using a set of 4 sensors around the dormitory.
2. The solution provides a mathematical modelling for the Humidity and Temperature levels for each Local and Remote locations. ```(HL: non-linear model)```
3. The solution provides a comparative analysis for the Humidity and Temperature levels for each Local and Remote locations including mean, standad deviation, minimum, maximum, and median.
4. ```(HL)```The Local samples are posted to the remote server.
5. Create a prediction the subsequent 12 hours for both temperature and humidity.
6. A poster summarizing the visual representations, model and analysis is created. The poster includes a recommendation about healthy levels for Temperature and Humidity.

# Criteria B: Design

## System Diagram **HL**
![image](https://github.com/drPinzonISAK/unit2_2022/raw/main/sysdim_hl.png)

**Fig.2** shows the system diagram for the proposed solution (**HL**). The indoor variables will be measured using a Raspberry PI and four DHT11 sensors located inside a room. Four sensors are used to determine more precisely the physical values and include measurement uncertainty. The outdoor variables will be requested to the remote server using a GET request to the API of the server at ```192.168.6.147/readings```. The local values are stored in a CSV database locally and POST to the server using the API and TOKEN authentication. A laptop computer is used for remotely controlling the local Rasberry Pi using a Dekptop sharing application (VNC Viewer). (Optional) Data from the local raspberry is downloaded to the laptop for analysis and processing.

## Flow Diagram

![Data reader flowchart](https://user-images.githubusercontent.com/112055062/206885070-35118e83-c5e5-441c-a534-7958938f0b8d.jpg)

**Fig.3** The figure shows the flow diagram of the function that returns temperature and humidity data from the sensor. The function is called "data_reader". The function inputs a pin number then uses data from the Adafruit_DHT.DHT11 which is used with the model of the sensor which is DHT11. Once the function gets the sensor and the pin number, it returns the temperature and humidity.

## Test Plan
| Description | Type | Inputs | Outputs | 
| ----------- | ---- | ------ | ------- |
**Table 1** 

## Record of Tasks
| Task No | Planned Action                                                | Planned Outcome                                                                                                 | Time estimate | Target completion date | Criterion |
|---------|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|---------------|------------------------|-----------|
| 1       | Write the Problem context | To have a clear idea on how to solve the problem and the best tools for the project                     | 10min         | Nov 22                 | A         |
| 2       | Connect to Rasbperry pi   |  To start learning how to use the raspeberry pi                      | 30min         | Nov 20                 | C         |
| 3       | Draw  the system diagram |  To have a clear idea of the hardware and software requirements for the proposal solution                      | 30min         | Nov 23                 | A         |
| 4 |Code the program to get the data | To be able to control and get data from the sensor | 1 hr | Dec 8| C|
| 5 | Connect the raspberry pi and the sensor | To get temperature and humidity from the sensor | 1hr | Dec 8 | C|
|6| Create flowchart for getting temperature and humidity data| To illustrate the code in a clearer picture|20 min| Dec11| B|

**Table 2** This table shows the record of tasks from the begining of creating this project until the end when the temperature and humidity is received and the analysis is completed. The table consists of the task number, planned action, planned outcome, the time taken to do the task, the date, and the criterion of the task(A,B,C,D).


# Criteria C: Development

## List of techniques used

## Development


# Criteria D: Functionality

A 7 min video demonstrating the proposed solution with narration
