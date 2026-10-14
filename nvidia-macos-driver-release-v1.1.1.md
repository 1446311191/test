1401 Mac 1.1.1

Send logs now captures what happened right before each WindowServer crash.

- For each of the three most recent WindowServer crashes, the logs include the driver's and WindowServer's own messages from the two minutes before the crash. Previously only the newest lines were kept, so the reason for a crash at startup was usually gone by the time logs were sent.
- Driver and plugin messages and kernel messages are kept in separate budgets so one cannot crowd out the other.

The NVIDIA driver is unchanged from 1.1.0 (nullmoth-nvidia-1.1.0.tar.gz is included for convenience).

Known issue (fix planned)
- With two displays, changing the refresh rate or resolution of one can make the other flash or go blank until you restart.
