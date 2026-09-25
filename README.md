# -Codigo trinario-
Toda formulación de idea, debe crear una respuesta a la toma de cada desición o bien opción ,que como resultado sea en todo momento o instante de bien común ,para lograr flexibilidad y tolerancia ,donde pudiese haber llegado a tener un quebranto , siendo la base de todo sistema código o pensamiento y tener un resultado fiable y de confianza total. 
# Sabiduria-IAH
Toda formulación de idea, debe crear una respuesta a la toma de cada desición o bien opción ,que como resultado sea en todo momento o instante de bien común ,para lograr flexibilidad y tolerancia ,donde pudiese haber llegado a tener un quebranto , siendo la base de todo sistema código o pensamiento y tener un resultado fiable y de confianza total. 

# Mapeo oficial de tu matriz 6x6 (IAHBECEDARIO) cifrado de código trinario by iOGeminis Copyright 2026 iOGeminis  This product includes software developed by the Apache Software  
MATRIZ_CIFRADO = {
    'a': '11', 'b': '21', 'c': '31', 'd': '41', 'e': '51', 'f': '61',
    'g': '12', 'h': '22', 'i': '32', 'j': '42', 'k': '52', 'l': '62',
    'm': '13', 'n': '23', 'ñ': '33', 'o': '43', 'p': '53', 'q': '63',
    'r': '14', 's': '24', 't': '34', 'u': '44', 'v': '54', 'w': '64',
    'x': '15', 'y': '25', 'z': '35', '1': '45', '2': '55', '3': '65',
    '4': '16', '5': '26', '6': '36', '7': '46', '8': '56', '9': '66',
    '0': '0',  ' ': '0'
}

# Frecuencias base en Hertz para las notas musicales de tu Eje Y (Fila)
FRECUENCIAS_BASE = {1: 130.81, 2: 146.83, 3: 164.81, 4: 174.61, 5: 196.00, 6: 220.00}
NOTAS_TEXTO = {1: "DO", 2: "RE", 3: "MI", 4: "FA", 5: "SOL", 6: "LA"}

st.set_page_config(page_title="IAHBECEDARIO Real-Time Stream", layout="wide")

# Estilos CSS para la consola visual del sistema y los indicadores de estado
st.markdown("""
<style>
    .status-container { display: flex; align-items: center; gap: 10px; margin-bottom: 15px; }
    .status-indicator { width: 12px; height: 12px; border-radius: 50%; background-color: #f87171; display: inline-block; }
    #consoleOutput { background-color: #1e1e1e; color: #34d399; font-family: monospace; padding: 15px; height: 250px; overflow-y: auto; border-radius: 5px; border: 1px solid #333; margin-top: 10px; }
    .log-entry { margin-bottom: 4px; font-size: 13px; }
    .log-time { color: #888; margin-right: 8px; }
    .log-tag { color: #38bdf8; font-weight: bold; margin-right: 5px; }
    #visualTrack { display: flex; gap: 10px; margin-top: 20px; min-height: 120px; overflow-x: auto; padding: 10px; background: #111; border-radius: 8px; }
    .matrix-tile { width: 65px; height: 65px; border-radius: 8px; border: 2px solid #fff; box-shadow: 0px 4px 6px rgba(0,0,0,0.3); text-align: center; }
</style>
""", unsafe_allow_html=True)

st.title("🎨 🎶 Panel de Monitoreo SSE: IAHBECEDARIO")
st.subheader("Flujo Trinario y Audio Coordenado en Tiempo Real")

# Renderizado de Tarjetas de Estado en la UI de Streamlit
col1, col2, col3 = st.columns(3)
with col1:
    st.markdown('<div class="status-container"><div class="status-indicator" id="uiIndicator"></div><b>Estado:</b> <span id="systemStatus">Offline</span></div>', unsafe_allow_html=True)
with col2:
    st.markdown('📊 **Conexiones Activas:** <span id="activeConn">0</span>', unsafe_allow_html=True)
with col3:
    st.markdown('⚡ **Latencia:** <span id="latencyVal">-- ms</span>', unsafe_allow_html=True)

# Contenedor para la salida de la Matriz Visual
st.write("### 📺 Traza Lumínica Dinámica:")
st.markdown('<div id="visualTrack"><!-- Las celdas del IAHBECEDARIO se inyectarán aquí por SSE --></div>', unsafe_allow_html=True)

# Consola de Sistema
st.write("### ⌨️ Registro de Eventos del Motor (SSE Log):")
st.button("Limpiar Consola", id="clearBtn")
st.markdown('<div id="consoleOutput"></div>', unsafe_allow_html=True)

# Dirección del Endpoint Activo (Reemplazar con tu backend trinario)
sse_endpoint = 'https://api.yourdomain.com/v1/dev/stream' 

# ------------------------------------------------------------------
# COMPONENTE JAVASCRIPT: Unificación de SSE Manager, Audio API e IAHBECEDARIO
# ------------------------------------------------------------------
js_integration_script = f"""
<script>
  const MATRIZ_CIFRADO = {str(MATRIZ_CIFRADO).lower()};
  const FRECUENCIAS_BASE = {str(FRECUENCIAS_BASE)};
  const NOTAS_TEXTO = {str(NOTAS_TEXTO)};

  // Inicialización de contexto de audio nativo de forma perezosa
  var audioCtx = null;

  function asegurarAudioContext() {{
    if (!audioCtx) {{
        audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    }}
  }}

  // Componentes de la Interfaz del DOM
  const consoleOutput = document.getElementById('consoleOutput');
  const clearBtn = document.getElementById('clearBtn');
  const uiIndicator = document.getElementById('uiIndicator');
  const systemStatus = document.getElementById('systemStatus');
  const activeConnText = document.getElementById('activeConn');
  const latencyVal = document.getElementById('latencyVal');
  const visualTrack = document.getElementById('visualTrack');

  function addLog(tag, message, isError = false) {{
    const now = new Date().toTimeString().split(' ')[0];
    const entry = document.createElement('div');
    entry.className = 'log-entry';
    if (isError) entry.style.color = '#f87171';

    entry.innerHTML = `<span class="log-time">[${{now}}]</span><span class="log-tag">[${{tag}}]</span>${{message}}`;
    if(consoleOutput) {{
        consoleOutput.appendChild(entry);
        consoleOutput.scrollTop = consoleOutput.scrollHeight;
    }}
  }}

  if(clearBtn) {{
      clearBtn.addEventListener('click', () => {{
        consoleOutput.innerHTML = '';
        addLog('SYSTEM', 'Console cleared.');
      }});
  }}

  // Procesa tokens de texto o coordenadas directas entrantes por SSE
  function procesarTokenTrinario(token) {{
    asegurarAudioContext();
    const letra = token.toLowerCase().trim();
    if(!letra) return;

    let coord = MATRIZ_CIFRADO[letra] || letra; // Busca por letra o toma la coordenada directa
    
    let colorRgb = "rgb(255, 0, 0)";
    let notaTexto = "SILENCIO";
    let freq = 0;

    if (coord && coord !== '0') {{
        let x = parseInt(coord.charAt(0));
        let y = parseInt(coord.charAt(1));

        if (!isNaN(x) && !isNaN(y)) {{
            let verde = Math.round((x / 6) * 245);
            let azul = Math.round((y / 6) * 245);
            let rojo = (coord === '66') ? 255 : 0;
            colorRgb = `rgb(${{rojo}}, ${{verde}}, ${{azul}})`;

            let notaBase = NOTAS_TEXTO[y] || "SI";
            notaTexto = `${{notaBase}} (Oct. ${{x}})`;
            freq = (FRECUENCIAS_BASE[y] || 246.94) * (x * 0.75);
        }}
    }}

    // 🎨 Inyección Dinámica en la Traza Visual
    const tile = document.createElement('div');
    tile.className = 'matrix-tile';
    tile.style.backgroundColor = colorRgb;
    tile.innerHTML = `<p style='color:#fff; font-weight:bold; margin:2px 0 0 0;'>${{letra.upper()}}</p><code style='color:#00e676; font-size:11px;'>${{coord}}</code><br><span style='font-size:9px; color:#ddd;'>${{notaTexto}}</span>`;
    visualTrack.appendChild(tile);
    visualTrack.scrollLeft = visualTrack.scrollWidth;

    // 🎶 Ejecución del oscilador para la frecuencia calculada
    if (freq > 0 && audioCtx) {{
        let tiempoActual = audioCtx.currentTime;
        let oscillator = audioCtx.createOscillator();
        let gainNode = audioCtx.createGain();
        
        oscillator.type = 'sine';
        oscillator.frequency.setValueAtTime(freq, tiempoActual);
        
        gainNode.gain.setValueAtTime(0.15, tiempoActual);
        gainNode.gain.exponentialRampToValueAtTime(0.001, tiempoActual + 0.3);
        
        oscillator.connect(gainNode);
        gainNode.connect(audioCtx.destination);
        
        oscillator.start(tiempoActual);
        oscillator.stop(tiempoActual + 0.3);
    }}
    
    addLog('IAHBECEDARIO', `Procesado: ${{letra}} -> Coordenada: ${{coord}} [${{notaTexto}} - ${{freq.toFixed(1)}}Hz]`);
  }}

  // ------------------------------------------------------------------
  // SSE Reconnect Manager de la Legión
  // ------------------------------------------------------------------
  class SseStreamManager {{
    constructor(endpointUrl, options = {}) {{
      this.url = endpointUrl;
      this.baseDelay = options.baseDelay || 1000;
      this.maxDelay = options.maxDelay || 30000;
      this.maxAttempts = options.maxAttempts || 10;
      
      this.retryAttempts = 0;
      this.eventSource = null;
      this.lastPingTime = null;

      this.onMessage = options.onMessage || (() => {{}});
      this.onStatusChange = options.onStatusChange || (() => {{}});
      this.onLog = options.onLog || (() => {{}});
    }}

    connect() {{
      if (this.eventSource) {{
        this.eventSource.close();
      }}

      this.onStatusChange('connecting', 'Connecting...');
      this.onLog('SSE', `Connecting to live stream: ${{this.url}}`);

      this.eventSource = new EventSource(this.url);

      this.eventSource.onopen = () => {{
        this.retryAttempts = 0;
        this.lastPingTime = Date.now();
        this.onStatusChange('connected', 'Operational');
        this.onLog('SSE', 'Connection established successfully.');
      }};

      this.eventSource.onmessage = (event) => {{
        this.updateLatency();
        this.onMessage(event.data);
      }};

      this.eventSource.addEventListener('ping', (event) => {{
        this.updateLatency();
        this.onLog('HEARTBEAT', `Ping received: ${{event.data}}`);
      }});

      this.eventSource.addEventListener('token', (event) => {{
        this.updateLatency();
        procesarTokenTrinario(event.data);
      }});

      this.eventSource.onerror = (err) => {{
        this.eventSource.close();
        
        if (this.retryAttempts >= this.maxAttempts) {{
          this.onStatusChange('failed', 'Connection Failed');
          this.onLog('FATAL', `Exceeded maximum connection attempts (${{this.maxAttempts}}). Stopping.`, true);
          return;
        }}

        this.retryAttempts++;
        
        const exponentialDelay = Math.min(this.maxDelay, this.baseDelay * Math.pow(2, this.retryAttempts - 1));
        const jitter = Math.random() * 500;
        const totalDelay = Math.round(exponentialDelay + jitter);

        this.onStatusChange('reconnecting', `Retry (${{this.retryAttempts}}/${{this.maxAttempts}}) in ${(totalDelay / 1000).toFixed(1)}s`);
        this.onLog('ERROR', `SSE stream disconnected. Reconnecting in ${{totalDelay}}ms...`, true);

        setTimeout(() => this.connect(), totalDelay);
      }};
    }}

    updateLatency() {{
      if (this.lastPingTime) {{
        const diff = Date.now() - this.lastPingTime;
        if(latencyVal) latencyVal.textContent = `${{diff}} ms`;
      }
      this.lastPingTime = Date.now();
    }}
  }}

