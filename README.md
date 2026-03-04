# Custom Blender/Revit Control (Pro Micro + 2x2 Buttons + Encoder)

<details>
  <summary><b>Overview</b></summary>

Este proyecto es un controlador físico DIY (impreso en 3D) pensado para acelerar workflows en **Blender** y **Revit**, reemplazando combinaciones incómodas del teclado por botones dedicados y un knob. El objetivo es que al prender la compu y conectar el control, puedas trabajar al toque sin estirar la mano al teclado.

</details>

<details>
  <summary><b>Hardware Used</b></summary>

- **Pro Micro ATmega32U4 (5V/16MHz, USB-C, pre-soldered headers)**
- **2x2 scan button module** (pines: L1, L2, R1, R2)
- **Rotary Encoder KY-040** (pines: GND, +, SW, DT, CLK)
- Dupont jumper wires
- Carcasa impresa en 3D (en 2 piezas para reducir soportes y facilitar montaje)

</details>

<details>
  <summary><b>Key Achievements (What Works)</b></summary>

### ✅ 1) Controller detected correctly via USB
El Pro Micro se configuró como **Arduino Leonardo** en Arduino IDE y fue detectado por macOS a través del puerto `/dev/cu.usbmodem…`.

### ✅ 2) 2x2 buttons module fully working
Se conectó el módulo 2x2 como matriz y se verificó con un test en Serial Monitor (115200 baud), confirmando lecturas estables por botón.

**Pin map (buttons):**
- L1 → D2  
- L2 → D3  
- R1 → D4  
- R2 → D5  

### ✅ 3) KY-040 encoder working (rotation + click)
Se cableó el encoder con alimentación correcta (VCC/GND) y señales en pines digitales. Se solucionó un cruce de pines (SW vs CLK/DT) usando pruebas en Serial Monitor y reordenando conexiones.

**Pin map (encoder):**
- CLK → D6  
- DT  → D7  
- SW  → D8  
- +   → VCC  
- GND → GND  

### ✅ 4) Blender macros working (keyboard + mouse HID)
Se reemplazaron los tests de Serial por un sketch que convierte el Pro Micro en **teclado/mouse HID**:
- 4 botones mapeados a:
  - **Shift + D** (Duplicate)
  - **G** (Grab)
  - **R** (Rotate)
  - **S** (Scale)
- Knob:
  - **Click → Spacebar**
  - **Rotate → Mouse wheel scroll** (usado como zoom en Blender)

Se confirmó que el control envía teclas correctamente (ejemplo visible en un campo de texto: `Drsg Drsg`), lo que demuestra que el dispositivo está generando inputs reales.

</details>

<details>
  <summary><b>How We Achieved It (Process Summary)</b></summary>

1. **Diseño 3D**: se trabajó con placeholders internos para reservar espacio para módulos + canales de cables, y luego se ajustó a una versión más compacta al notar que la impresión quedó grande.  
2. **Montaje seguro**: se definió uso de carcasa en 2 partes y soportes internos (standoffs) para sostener PCBs sin presión en componentes.  
3. **Bring-up eléctrico por etapas**:
   - Primero: Pro Micro solo (detección por USB)
   - Luego: botones 2x2 (test de matriz)
   - Luego: encoder (test de CLK/DT/SW + corrección de cableado)
4. **Programación en Arduino IDE**:
   - Test de botones → Serial Monitor
   - Test de encoder → Serial Monitor
   - Código combinado → Serial Monitor
   - Finalmente: sketch HID para Blender (teclado + rueda del mouse)
5. **Debugging práctico**:
   - Se corrigió el problema de Serial Monitor “Not connected”
   - Se corrigió cableado invertido del encoder (click registraba LEFT/RIGHT, giro registraba CLICK)
   - Se confirmó el mapeo final con pruebas reales

</details>

<details>
  <summary><b>Wiring (Pin Map)</b></summary>

### Buttons (2x2 module)
- L1 → D2  
- L2 → D3  
- R1 → D4  
- R2 → D5  

### Encoder (KY-040)
- CLK → D6  
- DT  → D7  
- SW  → D8  
- +   → VCC  
- GND → GND  

</details>

<details>
  <summary><b>Notes / Best Practices</b></summary>

- **Todo comparte GND** (tierra común).
- Encoder se alimenta con **VCC**, no RAW.
- Conectar/desconectar cables siempre con USB desconectado.
- Confirmar pins con Serial Monitor antes de cerrar la carcasa.
- Si el zoom del knob queda invertido, se puede invertir el signo del scroll o intercambiar CLK/DT.

</details>

<details>
  <summary><b>Troubleshooting</b></summary>

- **Serial Monitor “Not connected”**: selecciona el puerto correcto (`/dev/cu.usbmodem…`) y el board **Arduino Leonardo**.
- **Click imprime LEFT/RIGHT**: SW está conectado a CLK/DT por error.
- **Girar imprime CLICK**: SW y CLK/DT están cruzados.
- **Zoom invertido**: intercambia **CLK ↔ DT** o invierte el signo del scroll en el código.

</details>

<details>
  <summary><b>Next Steps</b></summary>

- Agregar joystick analógico (KY-023) para navegación o acciones extra.
- Migrar a **QMK + VIA** para remapeo fácil y shareable (con QR a instrucciones en GitHub).
- Crear capas (Blender/Revit) para cambiar perfiles sin reflashear.
- Refinar ergonomía: mango más compacto, redondeos, y mejor acceso al USB.

</details>


------ qmk for profiles -------------
here will use terminal for mac to instal qmk repositories and get ready
the plan is to have two profiles which is for Autodesk Revit y Blender
after Ill set up one for general things like music or videos etc...

