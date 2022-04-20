# Hardware

# Logic Capture (sr file)

- Decode capture with sigrok-cli

```bash
sigrok-cli -i warmup_flag_logically.sr -O ascii -P uart:baudrate=9600 --channels D0=RX
```

- Convert hex to char

```python
string = ''
nextisgood = False
with open('outd0-1.txt','r') as file:
    for line in file.read().split('\n'):
        char = chr(int(line,16))
        string += char

print(string)
```