# 🐛 Guía de Debugging - OpenBlock Desktop

## 📋 **Logs Agregados al Sistema**

He agregado logs detallados en todo el flujo de ejecución del botón verde. Aquí está el mapeo completo:

### **1. Interfaz de Usuario (UI)**
- **Archivo**: `openblock-desktop/node_modules/openblock-gui/src/containers/green-flag-overlay.jsx`
- **Logs**: 
  - `🟢 [UI] Green flag button clicked!`
  - `🚀 [UI] Starting VM...`
  - `🎪 [UI] Calling vm.greenFlag()...`
  - `📡 [UI] PROJECT_START event received!`
  - `✅ [UI] Green flag click handled!`

### **2. VM Manager (Render Process)**
- **Archivo**: `openblock-desktop/node_modules/openblock-gui/src/lib/vm-manager-hoc.jsx`
- **Logs**:
  - `🎬 [VM-MANAGER] Component mounted`
  - `🔧 [VM-MANAGER] Initializing VM...`
  - `✅ [VM-MANAGER] VM initialized`
  - `🚀 [VM-MANAGER] Starting VM...`
  - `✅ [VM-MANAGER] VM started`
  - `📁 [VM-MANAGER] Loading project...`
  - `✅ [VM-MANAGER] Project loaded successfully`
  - `🎨 [VM-MANAGER] Drawing renderer...`

### **3. VM Listener (Event Handling)**
- **Archivo**: `openblock-desktop/node_modules/openblock-gui/src/lib/vm-listener-hoc.jsx`
- **Logs**:
  - `📡 [VM-LISTENER] PROJECT_START event received!`

### **2. Máquina Virtual (VM)**
- **Archivo**: `openblock-vm/src/virtual-machine.js`
- **Logs**:
  - `🎯 [VM] Virtual Machine greenFlag() called`
  - `🔄 [VM] Delegating to runtime.greenFlag()...`
  - `✅ [VM] Virtual Machine greenFlag() completed`

### **3. Runtime - Motor de Ejecución**
- **Archivo**: `openblock-vm/src/engine/runtime.js`
- **Logs**:
  - `🚀 [RUNTIME] Green flag clicked! Starting execution...`
  - `🛑 [RUNTIME] Stopping all existing threads...`
  - `📡 [RUNTIME] Emitting PROJECT_START event...`
  - `⏰ [RUNTIME] Resetting project timer...`
  - `🎯 [RUNTIME] Clearing edge activated values for all targets...`
  - `🎭 [RUNTIME] Notifying X targets of green flag event...`
  - `📍 [RUNTIME] Notifying target X: TargetName`
  - `🎪 [RUNTIME] Starting hat blocks with event_whenflagclicked...`
  - `✅ [RUNTIME] Green flag execution completed!`

### **4. Sistema de Hilos (Threads)**
- **Archivo**: `openblock-vm/src/engine/runtime.js` (método startHats)
- **Logs**:
  - `🎪 [RUNTIME] startHats called with opcode: event_whenflagclicked`
  - `📋 [RUNTIME] Hat metadata: {...}`
  - `🧵 [RUNTIME] Found X new threads to start`
  - `🚀 [RUNTIME] Starting thread X/Y with topBlock: blockId`
  - `✅ [RUNTIME] Thread X started successfully`
  - `🎉 [RUNTIME] All X threads started!`

### **5. Ejecución de Bloques**
- **Archivo**: `openblock-vm/src/engine/execute.js`
- **Logs**:
  - `🔧 [EXECUTE] Executing block ID: blockId`

### **6. Bloques de Movimiento**
- **Archivo**: `openblock-vm/src/blocks/scratch3_motion.js`
- **Logs**:
  - `🚶 [MOTION] moveSteps: Moving X steps`
  - `📍 [MOTION] Current position: (x, y)`
  - `🧭 [MOTION] Current direction: X°`
  - `📐 [MOTION] Calculated movement: dx=X, dy=Y`
  - `🎯 [MOTION] New position: (x, y)`

## 🔍 **Cómo Ver los Logs**

### **Método 1: DevTools de Electron (Recomendado)**
1. **Abrir DevTools**: Presiona `Cmd+Option+I` (Mac) o `Ctrl+Shift+I` (Windows)
2. **Ir a Console**: Haz clic en la pestaña "Console"
3. **Hacer clic en el botón verde**: Verás todos los logs en tiempo real

### **Método 2: Terminal (Logs del proceso principal)**
Los logs también aparecerán en la terminal donde ejecutaste `npm start`

### **Método 3: Archivo de Logs**
Los logs se pueden redirigir a un archivo:
```bash
npm start 2>&1 | tee openblock-debug.log
```

## 🎯 **Flujo de Ejecución Esperado**

Cuando hagas clic en el botón verde, deberías ver esta secuencia en la consola del renderer:

```
🎬 [VM-MANAGER] Component mounted
🔧 [VM-MANAGER] Initializing VM...
✅ [VM-MANAGER] VM initialized
🚀 [VM-MANAGER] Starting VM...
✅ [VM-MANAGER] VM started
📁 [VM-MANAGER] Loading project...
✅ [VM-MANAGER] Project loaded successfully
🎨 [VM-MANAGER] Drawing renderer...

🟢 [UI] Green flag button clicked!
🚀 [UI] Starting VM...
🎪 [UI] Calling vm.greenFlag()...
📡 [VM-LISTENER] PROJECT_START event received!
📡 [UI] PROJECT_START event received!
✅ [UI] Green flag click handled!
```

**Nota**: Los logs del runtime (proceso principal) no aparecerán en la consola del renderer, pero el movimiento del sprite debería ser visible en el stage.

## 🛠️ **Solución de Problemas**

### **Si no ves logs:**
1. Verifica que la aplicación esté ejecutándose
2. Asegúrate de abrir DevTools (`Cmd+Option+I`)
3. Verifica que estés en la pestaña "Console"

### **Si los logs se cortan:**
1. Aumenta el buffer de la consola en DevTools
2. Usa el método de archivo de logs

### **Si hay errores:**
1. Revisa la pestaña "Console" en DevTools
2. Verifica que todos los archivos se hayan modificado correctamente

## 📝 **Próximos Pasos**

1. **Probar la aplicación**: Haz clic en el botón verde y observa los logs
2. **Verificar el movimiento**: El sprite debería moverse 10 pasos hacia arriba
3. **Analizar el flujo**: Observa cómo cada componente se comunica con el siguiente

## 🎉 **¡Listo para Debugging!**

Ahora tienes visibilidad completa del flujo de ejecución. Cada vez que hagas clic en el botón verde, verás exactamente qué está pasando en cada capa del sistema.
