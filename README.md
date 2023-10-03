# bug6_5.6.19
A kernel bug in usb_hcd_map_urb_for_dma in drivers/usb/core/hcd.c

Due to a specific malformed usb descriptor file, incorrect urb is constructed, causing the transfer buffer to be incorrectly located on the stack when the function usb_hcd_map_urb_for_dma (called by xhci_map_urb_for_dma , located in drivers/usb/core/hcd.c) submits the urb to the USB kernel, triggering a stack overflow and a crash of the linux kernel.

![image](https://github.com/wanrenmi/bug6_5.6.19/assets/42407501/57cd166d-6aee-4ab1-9fdd-7385dba5bc69)

The following is the input usb descriptor file:

![image](https://github.com/wanrenmi/bug6_5.6.19/assets/42407501/f9fa0bde-c496-4b36-a3ae-95eff620920a)

Among them, the first 18 bytes are the device descriptor, and the latter is the config descriptor. Insert the above file as a real or simulated USB device into a host using the linux kernel (It is currently certain that kernel version <=5.6.19 will be affected by this vulnerability. The result of the vulnerability triggering situation is shown in the figure below:

![image](https://github.com/wanrenmi/bug6_5.6.19/assets/42407501/4a61d6a8-a1b2-4234-915a-1556e278ead5)
