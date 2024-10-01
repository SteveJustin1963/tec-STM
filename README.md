# tec-STM scanning tunneling microscope

this STM project is easier to achive than the https://github.com/SteveJustin1963/tec-SEM


### MINT code for the STM controller.1 

explanations:

1. Removed unnecessary comments to make the code more concise.
2. Fixed the syntax for conditional statements. In MINT, conditions should be in parentheses.
3. Simplified some variable assignments and comparisons.
4. Removed the `.` (print) operations that were unnecessary.
5. In the `ENGAGE_SCANNER` function, combined conditions using `&` (AND) operator.
6. In the `STEP_MOTOR` function, removed explicit mention of input parameters as MINT uses stack-based parameter passing.
7. In the `IV_CURVE` function, used `BIAS` instead of `bias` for consistency (MINT is case-sensitive).
8. In the `RETRACT_Z` function, introduced `Z_TARGET` variable for clarity.
9. In the `PID_CONTROL` function, simplified calculations and used `+` instead of `!` for accumulating `INTEGRAL`.
10. In the `SCAN` function, added explicit increments for `X_SCAN` and `Y_SCAN`.
11. In the `MAIN_LOOP` function, simplified the infinite loop condition to `/T /W`.
12. Added `SETUP` and `MAIN_LOOP` calls at the end to start the program.

Note that some functions like `MOVE_MOTOR`, `MEASURE_CURRENT`, `RECORD_CURRENT_VOLTAGE`, `MOVE_Z_PIEZO`, `SCAN_PARAMETERS_INIT`, `NEXT_X_POSITION`, `RECORD_Z_POSITION`, `RETRACE_X_POSITION`, and `IDLE_FUNCTIONS` are assumed to be defined elsewhere in the system or in hardware interactions.

