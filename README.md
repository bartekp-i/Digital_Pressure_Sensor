# Digital Pressure Meter Console for Laboratory Gas Installations

This repository documents the design and implementation of a digital pressure meter console intended for use in laboratory gas systems. The project covers the complete development process, including electronic hardware design, firmware implementation, mechanical enclosure construction, and experimental validation through calibration and comparative measurements.

The system was developed to replace or complement traditional analog pressure meters by offering higher accuracy, digital data processing, and an intuitive user interface. The final device is capable of real-time pressure measurement, unit conversion, and calibrated output, making it suitable for laboratory environments where precision and repeatability are essential.

<p align="center">
 <img src="https://github.com/user-attachments/assets/c859e362-fbaf-4422-86e0-900e38efca52" width="400">
</p>

The architecture of the system is based on a precision pressure transducer whose analog output voltage is proportional to the applied pressure. This signal is first processed by an analog conditioning stage, where buffering and offset removal are applied to fully utilize the input range of the analog-to-digital converter. The conditioned signal is then sampled by a 16-bit external ADC operating over an SPI interface. Measurement data is transferred to an RP2040 microcontroller, which performs digital processing, calibration, and user interface handling before presenting the result on an OLED display.

<p align="center">
 <img src="https://github.com/user-attachments/assets/b163b677-a8bf-4a98-8c06-d5d4ee139371" width="700" height="230"/>
</p>

The hardware was designed with particular attention to signal integrity and measurement accuracy. The pressure sensor produces an output in the range of approximately 0.5 V to 4.5 V, which requires offset compensation to match the ADC reference voltage. This is achieved using a voltage follower and a differential amplifier configuration that removes the offset and rescales the signal to the effective measurement range. A precision 4.096 V voltage reference ensures stable ADC operation, while low-noise power supply filtering minimizes interference from external sources.

The central processing unit of the system is the RP2040 microcontroller, which manages communication with the ADC, the OLED display, and the user input buttons. The device can be powered either via USB-C or a DC barrel connector, both of which are filtered and regulated to provide stable operating conditions. USB-C is additionally used for firmware upload, debugging, and serial data logging.
<p align="center">
  <img width="850" height="690" alt="image" src="https://github.com/user-attachments/assets/2ff5cc4c-14e7-4ef8-b131-0c22e05c88d6" />
</p>

The printed circuit board was designed as a compact two-layer board with careful separation of analog and digital sections. Critical components such as the ADC, voltage reference, and decoupling capacitors are placed close to their respective loads to reduce noise and parasitic effects. The PCB layout follows good design practices for mixed-signal systems, including controlled routing for USB differential pairs and solid ground planes to ensure reliable operation.

<p align="center">
  <img width="989" height="276" alt="image" src="https://github.com/user-attachments/assets/e1a66a7b-3bc7-4fce-bd8b-6c77e6eb050f" />
</p>
<p align="center">
  <img width="350" height="550" alt="image" src="https://github.com/user-attachments/assets/251a65d2-ecdb-48d7-bef0-9a7e49355beb" />
  <img width="350" height="550" alt="image" src="https://github.com/user-attachments/assets/9b930df2-747d-4c0e-ade6-db4e567e0bab" />
</p>

To protect the electronics and improve ergonomics, a custom enclosure was designed and manufactured using 3D printing. The enclosure is made from PETG reinforced with carbon fiber, providing increased stiffness and thermal resistance compared to standard materials. The front panel is angled to improve OLED readability, and brass threaded inserts are embedded to allow repeated assembly without mechanical degradation. Neodymium magnets integrated into the base enable stable mounting on metal laboratory surfaces.

The firmware was written in MicroPython and is responsible for the complete data flow within the system. After initialization, the software continuously reads raw ADC values, converts them into voltage, applies digital filtering using a circular buffer, and calculates pressure values based on the selected operating mode. The firmware supports multiple pressure units and dynamically updates the display without visible flicker by using pre-rendered bitmap digits. User input buttons allow switching between measurement ranges and units during operation.

A critical part of the project was the experimental validation and calibration of the device. Measurements were conducted in a laboratory gas installation using a reference pressure sensor, the MKS 902B Absolute Piezo Vacuum Transducer. Pressure was gradually increased in controlled steps, and readings from the developed meter were compared against the reference values. Initial results showed good linearity but included systematic offset and gain errors.

<p align="center">
  <img width="1024" height="768" alt="image" src="https://github.com/user-attachments/assets/d68cf804-195c-402f-b535-43985660212d" />
</p>

Calibration was performed using the Ordinary Least Squares method to determine a linear correction function. The resulting calibration equation was implemented directly in the firmware, enabling real-time correction of measurement data. After calibration, the developed pressure meter showed very high agreement with the reference sensor across most of the tested range, achieving accuracy on the order of a few Torr and demonstrating excellent linearity.

<p align="center">
  <img width="768" height="472" alt="image" src="https://github.com/user-attachments/assets/30a3054c-8945-4785-9887-70fc0ee4cc78" />
</p>

The final results confirm that the developed system meets its functional assumptions and operates as a reliable digital pressure meter for laboratory use. The project demonstrates a complete measurement system, from analog signal acquisition through digital processing and calibration to user-friendly presentation. It also provides a solid foundation for future improvements, such as enhanced temperature compensation, extended pressure ranges, or standalone data logging capabilities.

This repository serves both as documentation of the finished device and as a reference for similar mixed-signal measurement system designs.

