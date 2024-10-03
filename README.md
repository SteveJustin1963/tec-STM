# tec-STM scanning tunneling microscope


### revsied STM Controller program in MINT 2.0 , implementation is a complete framework for the STM Controller. 

### changes and additions:
1. Constants: Added a CONSTANTS function to define important constants used throughout the program.
2. Variables: Initialized all necessary variables in the SETUP function.
3. ENGAGE_SCANNER: Modified to use MEASURE_CURRENT and compare with SETPOINT.
4. MOVE_MOTOR: Added a placeholder function for motor movement.
5. IV_CURVE: Implemented the I-V curve sweep, including DAC output for bias voltage.
6. RETRACT_Z: Implemented Z-piezo retraction.
7. PID_CONTROL: Kept as is, now integrated with MOVE_Z_PIEZO.
8. SCAN: Implemented full scanning functionality, including X and Y movement.
9. MEASURE_CURRENT: Added ADC reading logic with busy-wait.
10. MOVE_X_PIEZO, MOVE_Y_PIEZO, MOVE_Z_PIEZO: Implemented DAC output for each axis.
11. WRITE_DAC: Added basic SPI communication logic for DAC.
12. WAIT_TIME_STEP: Implemented a simple delay function.
13. IDLE_FUNCTIONS: Added placeholder for operations during idle time.


### further development needed:

1. SPI Communication: The actual SPI communication for DAC and ADC needs to be implemented based on your specific hardware.
2. Data Recording: The RECORD_Z_POSITION and RECORD_CURRENT_VOLTAGE functions need to be expanded to handle actual data storage.
3. Serial Communication: The SEND_LINE_DATA function needs to be implemented with your specific serial communication protocol.
4. Error Handling: Additional error checking and handling could be added to make the system more robust.
5. User Interface: Depending on your setup, you might want to add functions for user input and control, either through serial commands or other input methods.
6. Optimization: Depending on the performance requirements, some functions might need optimization for speed or memory usage.

