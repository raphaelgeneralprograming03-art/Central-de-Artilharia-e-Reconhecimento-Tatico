
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Central de Artilharia e Reconhecimento Tático (Kropyva-Link)</title>
    <style>
        :root {
            --bg-color: #030712;
            --panel-bg: rgba(15, 23, 42, 0.9);
            --border-color: #10b981;
            --accent-green: #22c55e;
            --accent-red: #ef4444;
            --accent-amber: #f59e0b;
            --accent-cyan: #06b6d4;
            --text-main: #e2e8f0;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Courier New', Courier, monospace;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            height: 100vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        header {
            background: linear-gradient(90deg, #020617, #0f172a, #020617);
            border-bottom: 2px solid var(--border-color);
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        header h1 {
            font-size: 1.05rem;
            color: var(--border-color);
            letter-spacing: 2px;
        }

        .badge-classification {
            background: var(--accent-red);
            color: #fff;
            padding: 4px 10px;
            font-size: 0.7rem;
            font-weight: bold;
            border-radius: 3px;
            letter-spacing: 1px;
            animation: pulse 2s infinite;
        }

        .main-container {
            display: flex;
            flex: 1;
            gap: 15px;
            padding: 15px;
            overflow: hidden;
        }

        /* Painel de Controle Esquerdo */
        .control-panel {
            width: 340px;
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            box-shadow: 0 0 15px rgba(16, 185, 129, 0.15);
        }

        .panel-section h3 {
            font-size: 0.8rem;
            color: var(--border-color);
            margin-bottom: 6px;
            border-bottom: 1px solid rgba(16, 185, 129, 0.3);
            padding-bottom: 3px;
            letter-spacing: 1px;
        }

        .btn-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        button {
            background: rgba(16, 185, 129, 0.1);
            border: 1px solid var(--border-color);
            color: var(--text-main);
            padding: 8px 10px;
            text-align: left;
            font-size: 0.75rem;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.3s ease;
        }

        button:hover {
            background: rgba(16, 185, 129, 0.3);
            color: #fff;
            box-shadow: 0 0 8px var(--border-color);
        }

        button.active {
            background: var(--border-color);
            color: var(--bg-color);
            font-weight: bold;
        }

        button.danger-btn {
            border-color: var(--accent-red);
            background: rgba(239, 68, 68, 0.1);
            color: #fca5a5;
        }
        button.danger-btn.active {
            background: var(--accent-red);
            color: #fff;
        }

        /* Área do Canvas Central */
        .viewport-container {
            flex: 1;
            background: #010409;
            border: 1px solid var(--border-color);
            border-radius: 6px;
            display: flex;
            flex-direction: column;
            position: relative;
            overflow: hidden;
        }

        .viewport-header {
            background: rgba(16, 185, 129, 0.05);
            padding: 6px 12px;
            font-size: 0.7rem;
            display: flex;
            justify-content: space-between;
            border-bottom: 1px solid rgba(16, 185, 129, 0.2);
        }

        canvas {
            flex: 1;
            display: block;
            width: 100%;
            height: 100%;
        }

        /* Painel de Telemetria Direito */
        .telemetry-panel {
            width: 290px;
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .metric-box {
            background: rgba(2, 6, 23, 0.7);
            border: 1px solid rgba(16, 185, 129, 0.3);
            border-radius: 4px;
            padding: 8px;
        }

        .metric-title {
            font-size: 0.65rem;
            color: var(--border-color);
            margin-bottom: 3px;
        }

        .metric-value {
            font-size: 1rem;
            font-weight: bold;
            color: #fff;
        }

        .status-ok { color: var(--accent-green); }
        .status-alert { color: var(--accent-red); }
        .status-warn { color: var(--accent-amber); }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.4; }
            100% { opacity: 1; }
        }
    </style>
</head>
<body>

    <header>
        <h1>CENTRAL DE DIREÇÃO DE FOGO & RECONHECIMENTO (KROPYVA-LINK)</h1>
        <div class="badge-classification">TEATRO DE OPERAÇÕES: UCRÂNIA</div>
    </header>

    <div class="main-container">
        <!-- Controles -->
        <div class="control-panel">
            <div class="panel-section">
                <h3>1. GUERRA ELETRÔNICA (EW)</h3>
                <div class="btn-group">
                    <button id="btn-ew" class="danger-btn" onclick="toggleEW()">Ativar Interferência GPS (Jamming)</button>
                </div>
            </div>

            <div class="panel-section">
                <h3>2. FLUXO SENSOR-TO-SHOOTER</h3>
                <div class="btn-group">
                    <button onclick="processCoordinates()">Processar Coordenadas na Central (NPU)</button>
                    <button onclick="transmitToKropyva()">Transmitir Overlay para Tablets (Kropyva)</button>
                    <button onclick="fireArtilleryStrike()">Disparar Obuseiro M777</button>
                </div>
            </div>

            <div class="panel-section">
                <h3>3. GERENCIAMENTO DE ALVOS</h3>
                <div class="btn-group">
                    <button onclick="spawnNewTarget()">Gerar Novo Alvo FPV</button>
                    <button onclick="resetSimulation()">Reiniciar Sistema</button>
                </div>
            </div>

            <div class="panel-section">
                <h3>ARQUITETURA DE SEGURANÇA</h3>
                <div style="font-size: 0.7rem; line-height: 1.3; color: #94a3b8;">
                    • FPV: Vídeo Bruto (Sem Telemetria RF)<br>
                    • Bunker: Central de Cálculo R$ 300k<br>
                    • Campo: Celulares/Tablets Descartáveis
                </div>
            </div>
        </div>

        <!-- Visualizador Canvas -->
        <div class="viewport-container">
            <div class="viewport-header">
                <span id="view-status">STATUS: AGUARDANDO CAPTURAS DE VÍDEO DO DRONE FPV</span>
                <span id="latency-display">LINK STARLINK: ESTÁVEL (1.2 ms)</span>
            </div>
            <canvas id="simCanvas"></canvas>
        </div>

        <!-- Telemetria -->
        <div class="telemetry-panel">
            <div class="panel-section">
                <h3>TELEMETRIA DA CENTRAL</h3>
            </div>
            <div class="metric-box">
                <div class="metric-title">ESTADO DO GPS (DRONE)</div>
                <div class="metric-value status-ok" id="stat-gps">GPS ATIVO</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">PROCESSAMENTO ÓPTICO (NPU)</div>
                <div class="metric-value status-ok" id="stat-optical">AGUARDANDO DADOS</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">VENTO E METEOROLOGIA</div>
                <div class="metric-value" id="stat-wind">14 km/h [NOROESTE]</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">SISTEMA KROPYVA (TABLETS)</div>
                <div class="metric-value status-ok" id="stat-kropyva">AGUARDANDO OVERLAY</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">ALVOS NEUTRALIZADOS</div>
                <div class="metric-value" id="stat-targets">0</div>
            </div>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('simCanvas');
        const ctx = canvas.getContext('2d');

        let ewActive = false;
        let coordsProcessed = false;
        let overlayTransmitted = false;
        let targetsNeutralized = 0;

        let drone = { x: 250, y: 150 };
        let bunker = { x: 100, y: 350 };
        let artillery = { x: 180, y: 380 };
        let enemyTarget = { x: 550, y: 200, firing: true, flashTimer: 0 };
        let projectiles = [];

        function resizeCanvas() {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight - 30;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        function toggleEW() {
            ewActive = !ewActive;
            let btn = document.getElementById('btn-ew');
            if (ewActive) {
                btn.classList.add('active');
                document.getElementById('stat-gps').innerText = 'BLOQUEADO (SPOOFING)';
                document.getElementById('stat-gps').className = 'metric-value status-alert';
                document.getElementById('view-status').innerText = 'STATUS: GPS INTERFERIDO! CALCULANDO VIA RECONHECIMENTO ÓPTICO DA CENTRAL';
                document.getElementById('view-status').style.color = 'var(--accent-amber)';
            } else {
                btn.classList.remove('active');
                document.getElementById('stat-gps').innerText = 'GPS ATIVO';
                document.getElementById('stat-gps').className = 'metric-value status-ok';
                document.getElementById('view-status').innerText = 'STATUS: FLUXO DE VÍDEO FPV CONECTADO VIA STARLINK';
                document.getElementById('view-status').style.color = 'var(--text-main)';
            }
        }

        function processCoordinates() {
            coordsProcessed = true;
            document.getElementById('stat-optical').innerText = 'COORDS GERADAS (PRECISÃO 0.2m)';
            document.getElementById('stat-optical').className = 'metric-value status-ok';
            document.getElementById('view-status').innerText = 'STATUS: CENTRAL PROCESSOU BALÍSTICA, VENTO E DISTÂNCIA ÓPTICA';
        }

        function transmitToKropyva() {
            if (!coordsProcessed) {
                alert('Processe as coordenadas na central primeiro!');
                return;
            }
            overlayTransmitted = true;
            document.getElementById('stat-kropyva').innerText = 'OVERLAY ENVIADO AOS TABLETS';
            document.getElementById('stat-kropyva').className = 'metric-value status-ok';
            document.getElementById('view-status').innerText = 'STATUS: COMANDANTE DE ARTILHARIA RECEBEU DADOS NO TABLET KROPYVA';
        }

        function fireArtilleryStrike() {
            if (!overlayTransmitted) {
                alert('Transmita o overlay para os tablets em campo antes de disparar!');
                return;
            }
            projectiles.push({
                x: artillery.x,
                y: artillery.y,
                destX: enemyTarget.x,
                destY: enemyTarget.y,
                progress: 0
            });
            document.getElementById('view-status').innerText = 'STATUS: FOGO DE CONTRABATERIA DISPARADO CONTRA ALVO';
        }

        function spawnNewTarget() {
            enemyTarget.x = 350 + Math.random() * (canvas.width - 400);
            enemyTarget.y = 80 + Math.random() * (canvas.height - 200);
            coordsProcessed = false;
            overlayTransmitted = false;
            document.getElementById('stat-optical').innerText = 'AGUARDANDO DADOS';
            document.getElementById('stat-optical').className = 'metric-value';
            document.getElementById('stat-kropyva').innerText = 'AGUARDANDO OVERLAY';
            document.getElementById('stat-kropyva').className = 'metric-value';
            document.getElementById('view-status').innerText = 'STATUS: NOVO ALVO FPV DETECTADO';
        }

        function resetSimulation() {
            projectiles = [];
            targetsNeutralized = 0;
            coordsProcessed = false;
            overlayTransmitted = false;
            document.getElementById('stat-targets').innerText = targetsNeutralized;
            document.getElementById('stat-optical').innerText = 'AGUARDANDO DADOS';
            document.getElementById('stat-optical').className = 'metric-value';
            document.getElementById('stat-kropyva').innerText = 'AGUARDANDO OVERLAY';
            document.getElementById('stat-kropyva').className = 'metric-value';
            spawnNewTarget();
        }

        // Loop Principal de Renderização Canvas 2D
        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // 1. Desenhar grade do teatro de operações
            ctx.strokeStyle = 'rgba(16, 185, 129, 0.05)';
            ctx.lineWidth = 1;
            let gridSize = 50;
            for (let x = 0; x < canvas.width; x += gridSize) {
                ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
            }
            for (let y = 0; y < canvas.height; y += gridSize) {
                ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
            }

            // 2. Movimento leve do Drone FPV
            drone.x += (Math.sin(Date.now() / 1000) * 0.5);
            drone.y += (Math.cos(Date.now() / 1000) * 0.3);

            // 3. Linha de transmissão de vídeo FPV -> Bunker (Starlink)
            ctx.strokeStyle = 'rgba(6, 182, 212, 0.4)';
            ctx.setLineDash([3, 3]);
            ctx.beginPath();
            ctx.moveTo(drone.x, drone.y);
            ctx.lineTo(bunker.x, bunker.y);
            ctx.stroke();

            // Linha de transmissão Central -> Tablets de Campo (Kropyva)
            if (overlayTransmitted) {
                ctx.strokeStyle = 'rgba(34, 197, 94, 0.7)';
                ctx.beginPath();
                ctx.moveTo(bunker.x, bunker.y);
                ctx.lineTo(artillery.x, artillery.y);
                ctx.stroke();
            }
            ctx.setLineDash([]); // Reset

            // 4. Desenhar Alvo Inimigo
            enemyTarget.flashTimer += 0.1;
            let isFlashing = Math.sin(enemyTarget.flashTimer) > 0;
            ctx.fillStyle = isFlashing ? '#ef4444' : '#b91c1c';
            ctx.beginPath();
            ctx.arc(enemyTarget.x, enemyTarget.y, 14, 0, Math.PI * 2);
            ctx.fill();

            ctx.fillStyle = '#fff';
            ctx.font = '10px Courier New';
            ctx.fillText('POSIÇÃO INIMIGA [ALVO FPV]', enemyTarget.x + 18, enemyTarget.y + 4);

            // Se coordenadas processadas, desenhar retícula óptica de mira
            if (coordsProcessed) {
                ctx.strokeStyle = '#10b981';
                ctx.lineWidth = 1.5;
                ctx.beginPath();
                ctx.arc(enemyTarget.x, enemyTarget.y, 25, 0, Math.PI * 2);
                ctx.stroke();
            }

            // 5. Desenhar Bunker (Central de Cálculo)
            ctx.fillStyle = '#06b6d4';
            ctx.fillRect(bunker.x - 12, bunker.y - 12, 24, 24);
            ctx.fillText('BUNKER SEGURO (CENTRAL NPU)', bunker.x - 20, bunker.y - 18);

            // 6. Desenhar Bateria de Artilharia & Tablets de Campo
            ctx.fillStyle = '#22c55e';
            ctx.fillRect(artillery.x - 10, artillery.y - 10, 20, 20);
            ctx.fillText('TRINCHEIRA / TABLET KROPYVA', artillery.x - 20, artillery.y + 25);

            // 7. Desenhar Drone FPV
            ctx.fillStyle = '#f59e0b';
            ctx.beginPath();
            ctx.arc(drone.x, drone.y, 6, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillText('DRONE FPV (VÍDEO BRUTO)', drone.x + 10, drone.y - 6);

            // 8. Atualizar e Desenhar Projéteis de Artilharia
            for (let i = projectiles.length - 1; i >= 0; i--) {
                let p = projectiles[i];
                p.progress += 0.025;
                let curX = p.x + (p.destX - p.x) * p.progress;
                let curY = p.y + (p.destY - p.y) * p.progress;

                ctx.fillStyle = '#f59e0b';
                ctx.beginPath();
                ctx.arc(curX, curY, 4, 0, Math.PI * 2);
                ctx.fill();

                if (p.progress >= 1) {
                    projectiles.splice(i, 1);
                    targetsNeutralized++;
                    document.getElementById('stat-targets').innerText = targetsNeutralized;
                    document.getElementById('view-status').innerText = 'STATUS: ALVO NEUTRALIZADO COM SUCESSO POR CONTRABATERIA!';
                }
            }

            requestAnimationFrame(animate);
        }

        animate();
    </script>
</body>
</html>
