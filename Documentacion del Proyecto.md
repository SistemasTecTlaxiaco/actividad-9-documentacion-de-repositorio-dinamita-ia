# 📸 Evidencia de Ejecución y Flujo de Transacción - Stellar Testnet

---

## 1. 🖥️ Interfaz principal del sistema

<img width="1600" height="933" alt="image" src="https://github.com/user-attachments/assets/c39156f6-d035-4e41-9851-7837aeb41021" />


### Descripción

Se muestra la interfaz principal del sistema **Stellar Payment**, donde:

- La wallet está conectada mediante **Freighter**
- Se visualiza el balance disponible: `10000.0000 XLM`
- El usuario puede:
  - Enviar XLM
  - Consultar historial

### Proceso

1. El usuario conecta su wallet.
2. El sistema obtiene la dirección pública.
3. Se consulta el balance desde Horizon API.
4. Se habilita el formulario de envío.

---

## 2. ✍️ Proceso de envío de transacción
<img width="1600" height="933" alt="image" src="https://github.com/user-attachments/assets/c39156f6-d035-4e41-9851-7837aeb41021" />



### Descripción

El usuario completa el formulario:

- Dirección destino
- Cantidad (ejemplo: `10 XLM`)

Al enviar:

- El sistema genera un **XDR sin firmar**
- Se envía a **Freighter** para firma

### Proceso

1. Usuario ingresa datos
2. Frontend envía datos al backend
3. Backend valida cuentas en Horizon
4. Backend construye XDR
5. Frontend solicita firma a Freighter

---

## 3. 🔐 Firma de transacción en Freighter

*(Visible en la primera imagen, panel derecho)*

### Descripción

Freighter muestra:

- Red: Testnet
- Monto: `-10 XLM`
- Fee: `0.00001 XLM`

### Proceso

1. Usuario revisa detalles
2. Confirma la transacción
3. Freighter firma el XDR con la clave privada
4. Devuelve el XDR firmado al frontend

---

## 4. ✅ Transacción completada

<img width="1600" height="933" alt="image" src="https://github.com/user-attachments/assets/09b3585d-8616-4d42-bd25-f6523feb0b4a" />


### Descripción

- Se muestra confirmación de envío exitoso
- Se genera un **Transaction Hash**
- Se incluye enlace a explorador

Balance actualizado:
- Antes: `10000 XLM`
- Después: `9990 XLM`

### Proceso

1. Backend recibe XDR firmado
2. Se envía a Horizon API
3. La red Stellar valida la transacción
4. Se retorna el hash al frontend
5. Se actualiza la UI

---

## 5. 🌐 Verificación en Stellar Expert

<img width="1600" height="933" alt="image" src="https://github.com/user-attachments/assets/be075c7b-9d9c-4a6b-8fe9-96d0326409e4" />


### Descripción

Se visualiza la transacción en el explorador:

- Estado: `Successful`
- Ledger registrado
- Cuenta origen y destino
- Firma digital incluida

### Proceso

1. Usuario accede al link generado
2. Consulta detalles en tiempo real
3. Verifica que la transacción está en blockchain

---

## 6. ⚙️ Código Backend (Configuración y Endpoint)

### Código reescrito

```javascript
// Configuración de Stellar Testnet
const HORIZON_URL = process.env.HORIZON_URL || 'https://horizon-testnet.stellar.org';
const NETWORK_PASS = process.env.NETWORK_PASS || 'Test SDF Network ; September 2015';
const PORT = process.env.PORT || 3001;

// Inicializar servidor de Horizon
const server = new Horizon.Server(HORIZON_URL, {
  timeout: 10000
});

// Configuración base
const BASE_FEE = '100'; // stroops
const TIMEOUT_SECONDS = 300; // 5 minutos

// Endpoint de salud del sistema
app.get('/api/health', (req, res) => {
  res.json({
    status: 'ok',
    network: 'testnet',
    horizon: HORIZON_URL,
    timestamp: new Date().toISOString()
  });
});
