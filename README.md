# Unit 2: A Distributed Weather Station for ISAK

# Criteria A: Planning

## Problem definition

Members of R3-14 house at UWC ISAK Japan in Karuizawa have rescued an injured young tree sparrow [^8] named ‘Peter’. Peter was found on the grass in front of the house unable to fly and can barely walk. The house members think that it is dangerous for Peter to be in the wild if he is still injured, so they are taking responsibility in taking care of Peter in a cardboard box and feeding him nuts. As Peter is recovering, he needs a specific condition of temperature and humidity to live in. Members of the house want a program to provide them with the accurate temperature and humidity in the room to be able to correctly control the condition for Peter to live in. 

## Proposed Solution
Considering the client requirements an adequate solution includes a low cost sensing device for humidity and temperature and a custom data script that process and anaysis the samples acquired. For a low cost sensing device an adequate alternative is the DHT11 sensor[^1] which is offered online for less than 5 USD and provides adequare precision and range for the client requirements (Temperature Range: 0°C to 50°C, Humidity Range: 20% to 90%). Similar devices such as the DHT22, AHT20 or the AM2301B [^9] have higher specifications, however the DHT11 uses a simple serial communication (SPI) rather than more eleborated protocols such as the I2C used by the alternatives. For the range, precision and accuracy required in this applicaiton the DHT11 provides the best compromise. Connecting the DHT11 sensor to a computer requires a device that provides a Serial Port communication. A cheap and often used alternative for prototyping is the Arduino UNO microcontroller [^3]. "Arduino is an open-source electronics platform based on easy-to-use hardware and software"[^4]. In additon to the low cost of the Arduino (< 6USD), this devide is programable and expandable[^1]. Other alternatives include diffeerent versions of the original Arduino but their size and price make them a less adequate solution.

Considering the budgetary constrains of the client and the hardware requirements, the software tool that I proposed for this solution is Python. Python is open source, it is mature and supported in mutiple platforms (platform-independent) including macOS, Windows, Linux and can also be used to program the Arduino microprocessor [^5][^6]. In comparison to the alternative C or C++, which share similar features, Python is a High level programming language (HLL) with high abstraction [^7]. For example, memory management is automatic in Python whereas it is responsability of the C/C++ developer to allocate and free up memory [^7], this could result in faster applications but also memory problems. In addition a HLL language will allow me and future developers extend the solution or solve issues proptly.  

## Design Statement
We will create a device that monitors the temperature and the humidity inside the room. A Raspberry Pi 4 [^2] and three DHT_11[^10] sensors will be used to collect the data. The data will then be uploaded to an online server in real time every 5 minutes for 48 hours. After the data is collected, we will compare the data of the temperature and humidity from what we collected indoors, and the temperature and humidity from outdoor sensors. After completing the comparison, we will provide a prediction for the future and give advice on how the client should adjust their room temperature and humidity for the health of the bird. We will take approximately 2 weeks to collect the data, provide predictions, and complete the success criteria for the client.

## Success Criteria

1. The solution provides a visual representation of the Humidity and Temperature values inside a dormitory (Local) and outside the house (Remote) for a period of minimum 48 hours. 
2. The local variables will be measured using a set of 3 sensors.
3. The solution provides a mathematical modelling for the Humidity and Temperature levels for each Local and Remote locations.
4. The solution provides a comparative analysis for the Humidity and Temperature levels for each Local and Remote locations including mean, standad deviation, minimum, maximum, and median.
5. ```(HL)```The Local samples are posted to the remote server.
6. Create a prediction the subsequent 12 hours for both temperature and humidity.
7. A poster summarizing the visual representations, model and analysis is created. The poster includes a recommendation about healthy levels for Temperature and Humidity.

# Criteria B: Design

## System Diagram **HL**

![IP  192 168 6 190](https://user-images.githubusercontent.com/112055062/206941441-e1749e4d-35b0-4b9e-a3cf-1ba261cdaba5.jpg)

**Fig.2** shows the system diagram for the proposed solution (**HL**). The indoor variables will be measured using a Raspberry PI and four DHT11 sensors located inside a room. Four sensors are used to determine more precisely the physical values and include measurement uncertainty. The outdoor variables will be requested to the remote server using a GET request to the API of the server at ```192.168.6.147/readings```. The local values are stored in a CSV database locally and POST to the server using the API and TOKEN authentication. A laptop computer is used for remotely controlling the local Rasberry Pi using a Dekptop sharing application (VNC Viewer). (Optional) Data from the local raspberry is downloaded to the laptop for analysis and processing.


## Flow Diagram

![Data reader flowchart](https://user-images.githubusercontent.com/112055062/206885070-35118e83-c5e5-441c-a534-7958938f0b8d.jpg)

**Fig.3** The figure shows the flow diagram of the function that returns temperature and humidity data from the sensor. The function is called "data_reader". The function inputs a pin number then uses data from the Adafruit_DHT.DHT11 which is used with the model of the sensor which is DHT11. Once the function gets the sensor and the pin number, it returns the temperature and humidity.

![smoothing diagram 2](https://user-images.githubusercontent.com/112055062/206913203-b6937bb0-f279-40e8-9708-cc9e12a775f4.jpg)

**Fig.4**  The figure shows the flow diagram of a function called “smoothing”. The function is used to smooth out the data which means that the function groups data next to each other according to the size window, finds the average, then puts it into a new list. The function inputs 2 things: the data list, and the size window. Then the mean of data from each window is calculated, and being put back to the list x and y. 

![flow diagram for get data outside](https://user-images.githubusercontent.com/112055062/207192925-4aef9a44-b765-4dd9-8407-85449d1e17f4.jpg)

**Fig.5** Shows the flow diagram of how the data was received from the remote sensor outdoors which its data is uploaded on an online API server http://192.168.6.142/readings. We requested the data by using the function get_sensor, then we created a list out of the data, and finally generated a diagram it. The program will generate two diagrams as Sensor_4 monitors the humidity of the remote location, and sensor_5 monitors the temperature. 


## Test Plan
| Description | Type | Inputs | Outputs | 
| ----------- | ---- | ------ | ------- |
|Check the diagrams for outside temperature and humidity | White Box, Functional, Unit testing| Run the code to show the graph | It shows the correct diagram|
|Check the diagrams for inside temperature and humidity |White Box, Functional, Unit testing | Run the code to show the graph | It shows the correct diagram|
|Check the diagram with error bars for inside temperature | White Box, Functional, Unit testing| Run the code to show the graph | It shows the correct diagram|
|Check the diagram with error bars for inside humidity |White box, functional, unit testing | Run the code to show the graph | It shows the correct diagram|
|Check the diagram for inside and outside temperature | White box, functional, unit testing | Run the code to show the graph | It shows the correct diagram|
|Check new_recording.py |Integration testing, functional, white box | Run the code to see if it sends new data for every 5 minutes | We saw that it stops after 20 minutes, so we added authentication line to while loop|
|Check the server | whitebox, functional, integration testing| Visit 192.168.6.142/readings to see if the server has our data | It shows all data we have sent|
|Check the diagrams for outside temperature and humidity by Rza | Black box, non-functional | Run the code to show the graph by someone as if he was the client | Black box, functional, integration testing |It shows the correct diagram|
|Check the diagrams for inside temperature and humidity by Rza |Black box, non-functional | Run the code to show the graph by someone as if he was the client | It shows the correct diagram|
|Check the diagram with error bars for inside temperature by Rza | Black box, non-functional | Run the code to show the graph by someone as if he was the client | It shows the correct diagram|
|Check the diagram with error bars for inside humidity by Rza |Black box, non-functional | Run the code to show the graph by someone as if he was the client | It shows the correct diagram|
|Check the diagram for inside and outside temperature by Rza |Black box, non-functional | Run the code to show the graph by someone as if he was the client | It shows the correct diagram|


**Table 1** The test plan shows how the program has been tested. The types of tests that are implemented are Unit Testing, Integration, and System Testing. The input in the table is the steps to follow in testing the functions and the output is what the program should return in the test. The test also include white box testing which is done by the developer who knows the code behind the program and black box testing done by other people who do not know the code.

## Record of Tasks
| Task No | Planned Action                                                | Planned Outcome                                                                                                 | Time estimate | Target completion date | Criterion |
|---------|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|---------------|------------------------|-----------|
| 1       | Write the Problem context | To have a clear idea on how to solve the problem and the best tools for the project                     | 10min         | Nov 22                 | A         |
| 2       | Connect to Rasbperry pi   |  To start learning how to use the raspeberry pi                      | 30min         | Nov 20                 | C         |
| 3       | Draw  the system diagram |  To have a clear idea of the hardware and software requirements for the proposal solution                      | 30min         | Nov 23                 | A         |
| 4 |Code the program to get the data | To be able to control and get data from the sensor | 1 hr | Dec 8| C|
| 5 | Connect the raspberry pi and the sensor | To get temperature and humidity from the sensor | 1hr | Dec 8 | C|
|6| Create flowchart for getting temperature and humidity data| To illustrate the code in a clearer picture|20 min| Dec11| B|
|7|Create test plan table| to let the users see that the functions are working| 10 min| Dec11 | B|
|8|Edit System Diagram | To show the client how the sensoring device works with all the elements from the program to the hardware| 20 min| 11 Dec | B|
|9|Create flowchart #2 for function smoothing | To show the client a clearer picture on how the code works | 20 min| Dec 12| B|
|10|Write Design Statement| To let the client understand the process of getting the solutin is like | 30 min| Dec 12|A|
|11| Write list of techniques used| To show what techniques were used in the design process of the product| 20 min | Dec 12 | D|
|12| Write the data storing method | To show how the data is being stored and to show the reliability and safety of the storing method | 30 min| Dec 12| B|
|12| Making a graph for temperature inside with error bars | To show how the data looks on a graph for better visualization for the client | 20 min | Dec 12| C|
|13| Making a graph for humidity inside with error bars | To show how the data looks on a graph for better visualization for the client | 20 min | Dec 12| C|
|14| Making a graph made of two graphs for temperature and humidity | To make it easier for the client to compare data inside and outside for Temperature and Humidity | 20 min | Dec 12 | C|
|15| Making a graph for temperature outside | To show how the data looks on a graph for better visualization for the client | 20 min | Dec 12| C|
|16| Making a graph for humidity outside | To show how the data looks on a graph for better visualization for the client | 20 min | Dec 12| C|
|17| Write the development section to fulfill success criteria 1| To show the clients what actions have been done to meet the needs of the client| 40 min| Dec 12|C|
|18| Write the development section to fulfill success criteria 2| To show the clients the method that is used in order to get 3 sensors| 30 min| Dec12| C|
|19| Write the development section to fulfill success criteria 3,4,5| To explain the work that has been done to complete the criteria| 3 hr| Dec 12 | C|
|20| Making a presentation | To show the client summary of the program | 10 min | Dec 13| D |



**Table 2** This table shows the record of tasks from the begining of creating this project until the end when the temperature and humidity is received and the analysis is completed. The table consists of the task number, planned action, planned outcome, the time taken to do the task, the date, and the criterion of the task(A,B,C,D).

## Data Storing Method
## 1. Online API Server
The data of temperature and humidity is being stored in an online API server: http://192.168.6.142/readings. Every 5 minutes for 48 hours, the program will send data to the server. As there are 3 sensors, each minute, the server will receive a total of 3 temperature data, and 3 humidity data. The data  will be sent to the server in a particular format. The following is an example of the 6 data sent each time:

```.py
{"value": 22.0, "id": 65010, "datetime": "2022-12-12T19:29:47.212069", "sensor_id": 507}, {"value": 20.0, "id": 65011, "datetime": "2022-12-12T19:29:47.260282", "sensor_id": 510}, {"value": 24.0, "id": 65012, "datetime": "2022-12-12T19:29:47.848937", "sensor_id": 508}, {"value": 20.0, "id": 65013, "datetime": "2022-12-12T19:29:47.918365", "sensor_id": 511}, {"value": 24.0, "id": 65014, "datetime": "2022-12-12T19:29:51.051459", "sensor_id": 509}, {"value": 20.0, "id": 65015, "datetime": "2022-12-12T19:30:02.040595", "sensor_id": 512},
```

Each data tells different information separated by commas, the first element, “value”, tells the value of either temperature(celcius) or humidity(%). The second element “id” shows the id of the data. The third element, “datetime”, tells the date (Year-Month-Day) and the time in hours, minutes, seconds, and milliseconds. The last element tells the sensor id of each of the 3 DHT 11 sensors and each sensor represents either temperature or humidity. We set sensor id 507-509 as the sensors that collect temperature, and the sensors 510-512 as the sensor for humidity.

For example, this data, {"value": 22.0, "id": 65010, "datetime": "2022-12-12T19:29:47.212069", "sensor_id": 507}, shows that the sensor detects temperature as the sensor id is “507”. It also shows that the temperature is 22 degrees Celsius, the id of this sensor is 65010, and the date of the data is 12th December 2022 at 7pm 29 minutes 47 seconds.

<img width="1249" alt="data in server" src="https://user-images.githubusercontent.com/112055062/207038574-7ef259ff-d995-48d2-9738-f786076aecda.png">

**Fig.6** Shows a section of the online API server http://192.168.6.142/readings where the data is being stored in real time every 5 minutes

## 2. Comma Separated Value File (CSV)
The data is not only stored in the online server, but also in a comma separated value file which we created called "Database_inside". The format of the data is organized to show the information of the datetime, the sensor id, and the value of either temperature of humidity from the local location indoors. 

![csv image](https://user-images.githubusercontent.com/112055062/207229610-c8bf7db0-0321-46a7-bd9c-5fd92ef6c091.png)

**Fig.7** Shows a screenshot of the CSV file, Database_inside which contains the information of the datetime, sensor id, and value of temperature and humidity.

# Criteria C: Development

## List of techniques used
1. Functions
2. For loop
3. While loop 
4. Creating graph using Matplotlib[^11]
5. Generating mathematical equations with Numpy [^12]
6. Working remotely with Raspberry pi 4 [^9] using VNC Viewer [^13]
7. Getting data from the sensors using Adafruit DHT Library [^14] 
8. Sending the temperature and humidity from the sensors to an online api server 
9. Validate integer input 
10. Using If statement
11. Creating ASCII art 
12. Using Variables
13. Importing functions from library
14. Print 
15. Using f strings
16. Using Elif statement
17. Using Else statement
18. Logging in to an online api server
19. Reading data from an online api server
20. Using ANSI colors 

## Development
The following are what we develope
