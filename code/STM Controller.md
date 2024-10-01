```


// MINT 2.0 Version of STM Controller for Z80-based System
// Scanning Tunneling Microscope Feedback and Scan Controller

// Pin and Variable Definitions
:SETUP
    `Initializing system...`
    // Pin assignments
    20 CS_DAC !
    17 LDAC !
    21 CS_ADC !
    4 CNV !
    3 BUSY !
    0 SERIAL_LED !
    1 TUNNEL_LED !

    // DAC and ADC channels
    2 DAC_CH_X !
    1 DAC_CH_Y !
    0 DAC_CH_Z !
    3 DAC_CH_BIAS !

    // Default settings
    100000 SCAN_SIZE !
    512 IMAGE_PIXELS !
    1 LINE_RATE !
    328 SETPOINT !
    328 BIAS !
    300000 KI !
    0 KP !

    /N `System setup complete.` /N
;

:ENGAGE_SCANNER
    `Engaging scanner...`
    0 engaged !
    /F pidEnabled !
    /F scanningEnabled !
    /F saturationCompensation !

    /U (
        error 0 < z -MAX_Z > & /W
        z ENGAGE_SCANNER_STEP_SIZE - z !
        WAIT_TIME_STEP
    )

    z -MAX_Z > (
        /T pidEnabled !
        /T engaged !
        /T saturationCompensation !
        /T TUNNEL_LED !
    ) /E (
        RETRACT
    )

    engaged
;

:STEP_MOTOR
    `Stepping motor...`
    // stepSize and direction should be on the stack
    MOVE_MOTOR
;

:IV_CURVE
    `Starting I-V curve sweep...`
    BIAS CURRENT_BIAS !
    /U (
        BIAS -CURRENT_BIAS > /W
        BIAS SWEEP_STEP_SIZE + BIAS !
        MEASURE_CURRENT
        RECORD_CURRENT_VOLTAGE
    )
    /N `I-V curve complete.` /N
;

:RETRACT_Z
    `Retracting Z-piezo...`
    RETRACT_DISTANCE Z_TARGET !
    /U (
        Z_POSITION Z_TARGET > /W
        MOVE_Z_PIEZO
        MEASURE_CURRENT
    )
    /N `Z-piezo retracted.` /N
;

:PID_CONTROL
    `Running PID control...`
    SETPOINT MEASURED_VALUE - ERROR !
    KP ERROR * PROPORTIONAL !
    KI ERROR * INTEGRAL + INTEGRAL !
    PROPORTIONAL INTEGRAL + OUTPUT !
    Z_POSITION OUTPUT + Z_POSITION !
;

:SCAN
    `Starting scan...`
    SCAN_PARAMETERS_INIT
    /U (
        Y_SCAN IMAGE_PIXELS < /W
        0 X_SCAN !
        /U (
            X_SCAN PIXELS_PER_LINE < /W
            NEXT_X_POSITION
            MEASURE_CURRENT
            PID_CONTROL
            RECORD_Z_POSITION
            X_SCAN 1 + X_SCAN !
        )
        RETRACE_X_POSITION
        Y_SCAN 1 + Y_SCAN !
    )
    /N `Scan complete.` /N
;

:MAIN_LOOP
    `Starting main control loop...`
    /U (
        /T /W  // Always true, runs indefinitely
        scanningEnabled (
            SCAN
        ) /E (
            IDLE_FUNCTIONS
        )
    )
;

// Start the program
SETUP
MAIN_LOOP

```
