##ARDUINO BRAIN LIBRARY

###OVERVIEW

Brain is an Arduino Library for parsing data from Neurosky-based EEG headsets.

It's designed to make it simple to send out an ASCII string of comma-separated values over serial, or to access processed brain wave information directly in your Arduino sketch. See the examples for code demonstrating each use case.

The `getCSV()` function returns a string (well, char*) listing the most recent brain data, in the following format:
**"signal strength, attention, meditation, delta, theta, low alpha, high alpha, low beta, high beta, low gamma, high gamma"
** Signal strength ranges from 0 - 200. Counterintuitively, 0 means the unit has connected successfully, and 200 means there is no signal.

The attention and meditation values both run from 0 - 100. Intuitively, higher numbers represent more attention or meditation.

The EEG power values — delta, theta, etc... - are a heavily filtered representation of the relative activity in different brain wavelengths. These values can not be mapped directly to physical values (e.g. volts), but are still of use when considered over time or relative to each other.

###EXAMPLES
Three examples are provided with the library:

- `BrainBlinker.ino` blinks an LED faster as your "attention" value increases.
- `BrainSerialTest.ino` prints out a CSV of brain data over serial.
- `BrainSoftSerialTest.ino` reads brain data over SoftwareSerial and prints a CSV over hardware serial. 

###FUNCTION OVERVIEW

- `Brain(HardwareSerial &_brainSerial)`  
Instantiates the brain library on a hardware serial port.

- `boolean update();`  
Call this in your main loop to read data from the headset. Returns true if there is a fresh packet.

- `char* readErrors();`  
Character string containing the most recent errors. Worth printing this out over serial if you're having trouble.

- `char* readCSV();`  
Character string with all of the latest brain values in a comma-delimited format. Intended to be printed over serial. The data is returned in this order: `signal strength, attention, meditation, delta, theta, low alpha, high alpha, low beta, high beta, low gamma, high gamma`


- `byte readSignalQuality();`  
Returns the latest signal quality reading. 200 is high, 0 is no signal. This (and the remainder of the functions) are mainly intended for use when you want the Arduino to use the brain data internally. (Saves you the hassle / memory expenditure of parsing the CSV.)

- `byte readAttention();`  
Returns the NeuroSky "eSense" attention value.

- `byte readMeditation();`  
Returns the NeuroSky "eSense" meditation value.

- `unsigned long* readPowerArray();`  
Returns an array of the eight power-band (FFT) values, in the same order as the CSV.

- `unsigned long readDelta();`  
Returns the delta (1-3Hz) power value, often associated with sleep.

- `unsigned long readTheta();`  
Returns the theta (4-7Hz) power value, associated with a relaxed, meditative state.

- `unsigned long readLowAlpha();`  
Returns the low alpha (8-9Hz) power value, higher when eyes are closed or you're relaxed/

- `unsigned long readHighAlpha();`  
Returns the high alpha (10-12Hz) power value.

- `unsigned long readLowBeta();`  
Returns the low beta (13-17Hz) power value, higher when you're alert and focused.

- `unsigned long readHighBeta();`  
Returns the high beta (18-30Hz) power value.

- `unsigned long readLowGamma();`  
Returns the low gamma (31-40Hz) power value, associated with multi-sensory processing.

- `unsigned long readMidGamma();`  
Returns the high gamma (41-50Hz) power value.
