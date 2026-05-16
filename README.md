# PSK and QPSK

### Aim
Write a simple Python program for the modulation and demodulation of PSK and QPSK.

### Tools required
* Laptop
* Python IDE

### Program
### PSK
```py
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter
def lowpass(x, fc, fs):
    b, a = butter(4, fc/(0.5*fs))
    return lfilter(b, a, x)
#Parameters
fs = 1000
fc = 50
bit_rate = 10
T = 1
t = np.linspace(0, T, fs)
# Message signal
bits = np.random.randint(0, 2, bit_rate)
bit_duration = fs // bit_rate
msg = np.repeat(bits, bit_duration)
# Carrier
carrier = np.sin(2*np.pi*fc*t)
# BPSK Modulation
psk_signal = np.sin(2*np.pi*fc*t + np.pi*msg)
# Demodulation
demodulated = psk_signal * carrier
filtered_signal = lowpass(demodulated, fc, fs)
decoded_bits = (filtered_signal[::bit_duration] < 0).astype(int)
# Plots
plt.figure(figsize=(10,8))
plt.subplot(4,1,1)
plt.plot(t, msg)
plt.title("Message Signal")
plt.grid()
plt.subplot(4,1,2)
plt.plot(t, carrier)
plt.title("Carrier Signal")
plt.grid()
plt.subplot(4,1,3)
plt.plot(t, psk_signal)
plt.title("PSK Modulated Signal")
plt.grid()
plt.subplot(4,1,4)
plt.step(range(len(decoded_bits)), decoded_bits)
plt.title("Demodulated Bits")
plt.grid()
plt.tight_layout()
plt.show()


```

### QPSK
```py
import numpy as np
import matplotlib.pyplot as plt
# Input symbols (bit pairs)
symbols = ['10', '11', '11', '10']
num_sym = len(symbols)
t = np.arange(-np.pi, np.pi, 0.1)
# QPSK phase signals
s00 = np.sin(t + np.pi/4)
s01 = np.sin(t + 3*np.pi/4)
s10 = np.sin(t + 5*np.pi/4)
s11 = np.sin(t + 7*np.pi/4)
# Modulation
mod_signal = []
input_bits = []
for sym in symbols:
    if sym == '00':
        mod_signal.extend(s00); input_bits.extend([0,0])
    elif sym == '01':
        mod_signal.extend(s01); input_bits.extend([0,1])
    elif sym == '10':
        mod_signal.extend(s10); input_bits.extend([1,0])
    else:
        mod_signal.extend(s11); input_bits.extend([1,1])
# Input waveform
inp_time = np.repeat(np.arange(len(input_bits)), 2)
inp_wave = np.repeat(input_bits, 2)
# Demodulation (decision by sample value)
demod_bits = []
sample_pt = 2
for i in range(num_sym):
    val = mod_signal[i*len(t) + sample_pt]
    if val <= -0.77:
        demod_bits.extend([0,0])
    elif val < -0.63:
        demod_bits.extend([0,1])
    elif val >= 0.77:
        demod_bits.extend([1,0])
    else:
        demod_bits.extend([1,1])

demod_time = np.repeat(np.arange(len(demod_bits)), 2)
demod_wave = np.repeat(demod_bits, 2)

# Plots
plt.figure(figsize=(10,6))
plt.subplot(3,1,1)
plt.plot(inp_time, inp_wave, drawstyle='steps-post')
plt.title("Input Binary Data")
plt.ylim(-0.5,1.5)
plt.grid()
plt.subplot(3,1,2)
plt.plot(mod_signal)
plt.title("QPSK Modulated Signal")
plt.grid()
plt.subplot(3,1,3)
plt.plot(demod_time, demod_wave, drawstyle='steps-post')
plt.title("Demodulated Signal")
plt.ylim(-0.5,1.5)
plt.grid()
plt.tight_layout()
plt.show()


```

### Output Waveform
### PSK waveform
<img width="1920" height="1014" alt="psk" src="https://github.com/user-attachments/assets/3798ef35-1222-480e-a69a-3a30bd594627" />


### QPSK waveform

<img width="1920" height="1014" alt="qpsk" src="https://github.com/user-attachments/assets/b32e5fc4-8450-47e0-96a0-2dee820e7c68" />


### Results

Thus the PSK (Phase Shift Keying) and QPSK (Quadrature Phase Shift Keying) are verified successfully using python.
