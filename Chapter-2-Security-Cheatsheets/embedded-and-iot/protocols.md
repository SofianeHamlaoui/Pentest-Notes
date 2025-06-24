# UART / Serial

* All clock speeds are set independently of the signal
   * `baud rate` is the rate at which bits will be transmitted (bits per second).
* There is one `start` bit.
* There is one `stop` bit (a low voltage cycle).
* 8 data bits in the middle.
* 1 parity bit at the end of the data.

**Settings**

Number of data bits/number of parity bits/number of stop bits
8/N/1 (8N1) means 8 data bits, No parity bit, and 1 stop bit

**Common Baud Rates**

* 9600
* 19200
* 38400
* 57600
* 115200

> Ref: Pentesting Hardware - A Practical Handbook by Mark C. https://github.com/unprovable/PentestHardware

- UART to Root [Pending]: https://exfil.co/2019/02/14/uart-to-root-the-harder-way/

# SPI

* `MISO - Master In Slave Out` - Data flow from the slave units to the master unit
* `MOSI - Master Out Slave In` - Data flow from the
master unit to the slave units
* `SCLK - The clock signal pin` - Rising edge of the clock triggers the level of MISO/MOSI to be read as the current bit by the target device.
* `SS/CS - Slave/Chip Select` - Select a device, this pin is grounded, telling the particular device to listen up.
* `VCC/GND`

**Typical**
![SPI](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/SPI_three_slaves.svg/545px-SPI_three_slaves.svg.png)
> https://en.wikipedia.org/wiki/Chip_select

**Daisy Chained**

* The SPI port of each slave is designed to send out during the second group of clock pulses an exact copy of the data it received during the first group of clock pulses.
* The whole chain acts as a communication shift register.
* Daisy chaining is often done with shift registers to provide a bank of inputs or outputs through SPI.
* Each slave copies input to output in the next clock cycle until active low SS line goes high.

![SPI](https://upload.wikimedia.org/wikipedia/commons/thumb/9/97/SPI_three_slaves_daisy_chained.svg/545px-SPI_three_slaves_daisy_chained.svg.png)

* SPI mode - is the combination of CPOL and CPHA
* CPOL - Polarity of the clock
   * CPOL=0 is a clock which idles at 0
      * Leading edge is a rising edge
      * Trailing edge is a falling edge
   * CPOL=1 is a clock which idles at 1
      * Leading edge is a falling edge
      * Trailing edge is a rising edge
* CPHA - Timing of the data bits relative to the clock pulses
   * CPHA=0
      * "out" side changes the data on the trailing edge of the preceding clock cycle, while the "in" side captures the data on (or shortly after) the leading edge of the clock cycle
      * First bit must be on the MOSI line before the leading clock edge
   * CPHA=1
      * "out" side changes the data on the leading edge of the current clock cycle, while the "in" side captures the data on (or shortly after) the trailing edge of the clock cycle
      * For the last cycle, the slave holds the MISO line valid until slave select is deasserted.

![CPOL / CPHA](https://upload.wikimedia.org/wikipedia/commons/thumb/6/6b/SPI_timing_diagram2.svg/645px-SPI_timing_diagram2.svg.png)
> https://en.wikipedia.org/wiki/Serial_Peripheral_Interface

# I2C

# JTAG

* `TDI - Test Data In`
* `TDO - Test Data Out` - When you daisychain IC’s with JTAG, the TDO of one goes to the TDI of the next, until it loops back to the debug header.
* `TCK - Test Clock` - JTAG clock signal, the rising edge triggering a read operation. TCK is not chained, but rather forms a ’test clock bus’ along with TMS each IC can see the clock and TMS signals.
* `TMS - Test Mode Select` - Read as the clock signal rises, and determines the next state of the internal JTAG controller
* `TRST - Test Reset` - An optional pin that can reset the internal test controller, but this isn’t required.


* Test Access Port (TAP) Controller to handle JTAG commands
* Minimally 3 registers (instruction register, 2 or more data registers)
* State-machine that uses the TMS level to decide what to do after each clock cycle
* TAP controller connects to the boundary cells
* Boundary Cells can raise/lower a leg’s voltage to influence the behavior of the chip

> Ref: Pentesting Hardware - A Practical Handbook by Mark C. https://github.com/unprovable/PentestHardware

# SWD
