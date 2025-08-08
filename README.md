<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GHOST PROTOCOL - Hacker Interface</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Orbitron', monospace;
            background: #000;
            color: #00ff41;
            overflow: hidden;
            cursor: none;
            user-select: none;
        }

        .matrix-rain {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            opacity: 0.15;
        }

        .main-interface {
            display: grid;
            grid-template-columns: 300px 1fr 350px;
            grid-template-rows: 80px 1fr 60px;
            height: 100vh;
            gap: 2px;
            background: #001100;
        }

        .header {
            grid-column: 1 / -1;
            background: linear-gradient(45deg, #003300, #001100);
            border-bottom: 2px solid #00ff41;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 30px;
            box-shadow: 0 0 20px rgba(0, 255, 65, 0.3);
        }

        .logo {
            font-size: 1.8rem;
            font-weight: 900;
            color: #00ff41;
            text-shadow: 0 0 10px #00ff41;
            letter-spacing: 3px;
        }

        .status-indicators {
            display: flex;
            gap: 20px;
            align-items: center;
        }

        .indicator {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.9rem;
        }

        .led {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            animation: pulse 2s infinite;
        }

        .led.green { 
            background: #00ff41; 
            box-shadow: 0 0 10px #00ff41;
        }
        .led.red { 
            background: #ff4141; 
            box-shadow: 0 0 10px #ff4141;
        }
        .led.blue { 
            background: #4169ff; 
            box-shadow: 0 0 10px #4169ff;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.3; }
        }

        .sidebar {
            background: rgba(0, 20, 0, 0.8);
            border-right: 1px solid #00ff41;
            padding: 20px;
            backdrop-filter: blur(5px);
        }

        .tool-section {
            margin-bottom: 30px;
        }

        .section-title {
            font-size: 1rem;
            font-weight: 700;
            margin-bottom: 15px;
            color: #00ff41;
            text-transform: uppercase;
            letter-spacing: 1px;
            border-bottom: 1px solid #003300;
            padding-bottom: 5px;
        }

        .tool-button {
            display: block;
            width: 100%;
            background: rgba(0, 255, 65, 0.1);
            border: 1px solid #00ff41;
            color: #00ff41;
            padding: 12px 15px;
            margin-bottom: 8px;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s;
            font-family: inherit;
            font-size: 0.85rem;
            text-align: left;
        }

        .tool-button:hover {
            background: rgba(0, 255, 65, 0.2);
            box-shadow: 0 0 15px rgba(0, 255, 65, 0.3);
            transform: translateX(5px);
        }

        .tool-button.active {
            background: rgba(0, 255, 65, 0.3);
            box-shadow: inset 0 0 10px rgba(0, 255, 65, 0.5);
        }

        .main-screen {
            background: rgba(0, 10, 0, 0.9);
            border: 1px solid #003300;
            padding: 30px;
            position: relative;
            overflow: hidden;
        }

        .screen-content {
            height: 100%;
            position: relative;
        }

        .data-stream {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            overflow-y: auto;
            font-size: 0.9rem;
            line-height: 1.6;
        }

        .info-panel {
            background: rgba(0, 30, 0, 0.8);
            border-left: 1px solid #00ff41;
            padding: 20px;
            backdrop-filter: blur(5px);
        }

        .target-info {
            background: rgba(0, 255, 65, 0.05);
            border: 1px solid #00ff41;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 20px;
        }

        .info-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-size: 0.85rem;
        }

        .progress-section {
            margin-bottom: 25px;
        }

        .progress-bar {
            width: 100%;
            height: 8px;
            background: rgba(0, 255, 65, 0.2);
            border-radius: 4px;
            overflow: hidden;
            margin-bottom: 10px;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #00ff41, #00cc33);
            border-radius: 4px;
            transition: width 0.5s ease;
            box-shadow: 0 0 10px rgba(0, 255, 65, 0.5);
        }

        .footer {
            grid-column: 1 / -1;
            background: linear-gradient(45deg, #001100, #003300);
            border-top: 2px solid #00ff41;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 30px;
            font-size: 0.8rem;
        }

        .footer-stats {
            display: flex;
            gap: 30px;
        }

        .custom-cursor {
            position: fixed;
            width: 24px;
            height: 24px;
            background: radial-gradient(circle, #00ff41, transparent);
            border: 2px solid #00ff41;
            border-radius: 50%;
            pointer-events: none;
            z-index: 9999;
            animation: cursorPulse 1.5s infinite;
        }

        @keyframes cursorPulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.2); }
        }

        .scan-line {
            position: absolute;
            left: 0;
            right: 0;
            height: 2px;
            background: linear-gradient(90deg, transparent, #00ff41, transparent);
            animation: scanLine 3s infinite;
        }

        @keyframes scanLine {
            0% { top: 0; opacity: 1; }
            100% { top: 100%; opacity: 0; }
        }

        .hexagon {
            width: 30px;
            height: 30px;
            background: #00ff41;
            position: relative;
            margin: 10px 0;
            clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
            animation: rotate 10s linear infinite;
        }

        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        .glitch-text {
            animation: glitchText 0.3s infinite;
        }

        @keyframes glitchText {
            0% { transform: translate(0); }
            20% { transform: translate(-1px, 1px); }
            40% { transform: translate(-1px, -1px); }
            60% { transform: translate(1px, 1px); }
            80% { transform: translate(1px, -1px); }
            100% { transform: translate(0); }
        }

        .network-map {
            position: relative;
            height: 200px;
            background: rgba(0, 255, 65, 0.02);
            border: 1px solid #003300;
            border-radius: 8px;
            margin: 15px 0;
            overflow: hidden;
        }

        .node {
            position: absolute;
            width: 20px;
            height: 20px;
            background: #00ff41;
            border-radius: 50%;
            animation: nodePulse 2s infinite;
            box-shadow: 0 0 10px #00ff41;
        }

        @keyframes nodePulse {
            0%, 100% { opacity: 0.5; transform: scale(1); }
            50% { opacity: 1; transform: scale(1.3); }
        }

        .connection-line {
            position: absolute;
            height: 1px;
            background: linear-gradient(90deg, transparent, #00ff41, transparent);
            animation: dataFlow 1.5s infinite;
        }

        @keyframes dataFlow {
            0% { opacity: 0; }
            50% { opacity: 1; }
            100% { opacity: 0; }
        }

        .data-packet {
            color: #00cc33;
            font-size: 0.75rem;
            opacity: 0.8;
        }

        .alert-box {
            background: rgba(255, 0, 0, 0.1);
            border: 1px solid #ff4141;
            border-radius: 5px;
            padding: 10px;
            margin: 10px 0;
            color: #ff4141;
            font-size: 0.8rem;
            animation: alertBlink 1s infinite;
        }

        @keyframes alertBlink {
            0%, 100% { opacity: 0.7; }
            50% { opacity: 1; }
        }

        @media (max-width: 1024px) {
            .main-interface {
                grid-template-columns: 250px 1fr 300px;
            }
        }

        @media (max-width: 768px) {
            .main-interface {
                grid-template-columns: 1fr;
                grid-template-rows: 60px 1fr 50px;
            }
            
            .sidebar, .info-panel {
                display: none;
            }
        }
    </style>
</head>
<body>
    <canvas class="matrix-rain" id="matrixCanvas"></canvas>
    <div class="custom-cursor" id="customCursor"></div>

    <div class="main-interface">
        <div class="header">
            <div class="logo">GHOST PROTOCOL</div>
            <div class="status-indicators">
                <div class="indicator">
                    <div class="led green"></div>
                    <span>ONLINE</span>
                </div>
                <div class="indicator">
                    <div class="led blue"></div>
                    <span>ENCRYPTED</span>
                </div>
                <div class="indicator">
                    <div class="led red"></div>
                    <span>STEALTH</span>
                </div>
            </div>
        </div>

        <div class="sidebar">
            <div class="tool-section">
                <div class="section-title">INTRUSION TOOLS</div>
                <button class="tool-button active" onclick="activateTool('scanner')">🔍 Port Scanner</button>
                <button class="tool-button" onclick="activateTool('exploit')">💣 Exploit Engine</button>
                <button class="tool-button" onclick="activateTool('bruteforce')">🔓 Brute Force</button>
                <button class="tool-button" onclick="activateTool('payload')">🚀 Payload Injector</button>
            </div>

            <div class="tool-section">
                <div class="section-title">DATA TOOLS</div>
                <button class="tool-button" onclick="activateTool('extractor')">📁 Data Extractor</button>
                <button class="tool-button" onclick="activateTool('decoder')">🔐 Hash Decoder</button>
                <button class="tool-button" onclick="activateTool('sniffer')">📡 Packet Sniffer</button>
                <button class="tool-button" onclick="activateTool('crypto')">🔒 Crypto Breaker</button>
            </div>

            <div class="tool-section">
                <div class="section-title">STEALTH MODE</div>
                <button class="tool-button" onclick="activateTool('vpn')">🌐 VPN Tunnel</button>
                <button class="tool-button" onclick="activateTool('trace')">👻 Anti-Trace</button>
                <button class="tool-button" onclick="activateTool('clean')">🧹 Log Cleaner</button>
            </div>

            <div class="hexagon"></div>
        </div>

        <div class="main-screen">
            <div class="scan-line"></div>
            <div class="screen-content">
                <div class="data-stream" id="mainOutput"></div>
            </div>
        </div>

        <div class="info-panel">
            <div class="target-info">
                <div class="section-title">TARGET INFO</div>
                <div class="info-row">
                    <span>IP ADDRESS:</span>
                    <span id="targetIP">192.168.1.100</span>
                </div>
                <div class="info-row">
                    <span>HOSTNAME:</span>
                    <span id="hostname">MAINFRAME-SRV</span>
                </div>
                <div class="info-row">
                    <span>OS:</span>
                    <span id="targetOS">WINDOWS 2019</span>
                </div>
                <div class="info-row">
                    <span>FIREWALL:</span>
                    <span id="firewall">ACTIVE</span>
                </div>
            </div>

            <div class="progress-section">
                <div class="section-title">OPERATION STATUS</div>
                <div style="margin-bottom: 10px;">
                    <div style="font-size: 0.8rem; margin-bottom: 5px;">INFILTRATION PROGRESS</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="infiltrationProgress" style="width: 0%"></div>
                    </div>
                </div>
                <div style="margin-bottom: 10px;">
                    <div style="font-size: 0.8rem; margin-bottom: 5px;">DATA EXTRACTION</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="extractionProgress" style="width: 0%"></div>
                    </div>
                </div>
                <div>
                    <div style="font-size: 0.8rem; margin-bottom: 5px;">STEALTH LEVEL</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="stealthProgress" style="width: 85%"></div>
                    </div>
                </div>
            </div>

            <div class="network-map" id="networkMap">
                <div class="section-title" style="position: absolute; top: 5px; left: 10px; font-size: 0.7rem;">NETWORK MAP</div>
            </div>

            <div class="alert-box" id="alertBox">
                ⚠️ SECURITY BREACH DETECTED - INITIALIZING COUNTERMEASURES
            </div>
        </div>

        <div class="footer">
            <div class="footer-stats">
                <span>TARGETS: <span id="targetCount">0</span></span>
                <span>EXPLOITS: <span id="exploitCount">0</span></span>
                <span>DATA: <span id="dataSize">0 MB</span></span>
            </div>
            <div>
                <span id="systemTime"></span>
            </div>
            <div>
                <button class="tool-button" onclick="toggleSound()" id="soundToggle" style="margin: 0; padding: 8px 15px; font-size: 0.7rem;">🔊 ASMR ON</button>
            </div>
        </div>
    </div>

    <script>
        let audioContext = null;
        let soundEnabled = true;
        let currentTool = 'scanner';
        let animationSpeed = 100;

        // Tool data and outputs
        const toolOutputs = {
            scanner: [
                '>>> INITIATING PORT SCAN SEQUENCE',
                '>>> TARGET: 192.168.1.100',
                '',
                '[SCAN] Probing port 21/tcp... CLOSED',
                '[SCAN] Probing port 22/tcp... OPEN (SSH-2.0-OpenSSH_8.0)',
                '[SCAN] Probing port 23/tcp... FILTERED',
                '[SCAN] Probing port 25/tcp... CLOSED',
                '[SCAN] Probing port 53/tcp... OPEN (DNS)',
                '[SCAN] Probing port 80/tcp... OPEN (Apache/2.4.41)',
                '[SCAN] Probing port 110/tcp... CLOSED',
                '[SCAN] Probing port 143/tcp... CLOSED',
                '[SCAN] Probing port 443/tcp... OPEN (OpenSSL)',
                '[SCAN] Probing port 993/tcp... CLOSED',
                '[SCAN] Probing port 995/tcp... CLOSED',
                '',
                '[RESULT] 4 open ports detected',
                '[VULN] SSH version vulnerable to CVE-2019-6111',
                '[VULN] Apache server outdated - multiple vulnerabilities',
                '',
                '>>> SCAN COMPLETE - PROCEEDING TO EXPLOITATION PHASE'
            ],
            exploit: [
                '>>> LOADING EXPLOIT FRAMEWORK',
                '>>> METASPLOIT v6.3.25-dev initialized',
                '',
                '[EXPLOIT] Loading module: exploit/linux/ssh/ssh_login_pubkey',
                '[EXPLOIT] Setting RHOSTS => 192.168.1.100',
                '[EXPLOIT] Setting RPORT => 22',
                '[EXPLOIT] Setting USERNAME => admin',
                '',
                '[PAYLOAD] Generating reverse shell payload...',
                '[PAYLOAD] Payload size: 1,247 bytes',
                '[PAYLOAD] Encoder: x86/shikata_ga_nai',
                '',
                '[EXECUTE] Launching exploit in 3... 2... 1...',
                '[SUCCESS] Shell session 1 opened (10.0.0.1:4444 -> 192.168.1.100:22)',
                '[ACCESS] Root privileges obtained',
                '',
                '>>> EXPLOITATION SUCCESSFUL - MAINTAINING PERSISTENCE'
            ],
            bruteforce: [
                '>>> INITIALIZING BRUTE FORCE ATTACK',
                '>>> TARGET SERVICE: SSH (port 22)',
                '',
                '[ATTACK] Dictionary: rockyou.txt (14,344,391 passwords)',
                '[ATTACK] Threads: 16',
                '[ATTACK] Rate: 1000 attempts/second',
                '',
                '[ATTEMPT] admin:password... FAILED',
                '[ATTEMPT] admin:123456... FAILED',
                '[ATTEMPT] admin:admin... FAILED',
                '[ATTEMPT] admin:letmein... FAILED',
                '[ATTEMPT] admin:welcome... FAILED',
                '[ATTEMPT] admin:monkey... FAILED',
                '[ATTEMPT] admin:dragon... FAILED',
                '[ATTEMPT] admin:sunshine... FAILED',
                '[ATTEMPT] admin:master... FAILED',
                '[SUCCESS] admin:P@ssw0rd123... AUTHENTICATED!',
                '',
                '>>> CREDENTIALS CRACKED - ACCESS GRANTED'
            ],
            payload: [
                '>>> PAYLOAD INJECTION INITIATED',
                '>>> CRAFTING CUSTOM SHELLCODE',
                '',
                '[BUILD] Generating polymorphic payload...',
                '[BUILD] Encoding with XOR cipher (key: 0xDEADBEEF)',
                '[BUILD] Adding NOP sled (128 bytes)',
                '[BUILD] Injecting return address: 0x7fffffff',
                '',
                '[INJECT] Targeting buffer overflow in service daemon',
                '[INJECT] Payload size: 512 bytes',
                '[INJECT] Delivery method: HTTP POST request',
                '',
                '[EXECUTE] Sending malicious payload...',
                '[SUCCESS] Code execution achieved!',
                '[SUCCESS] Reverse shell established',
                '[SUCCESS] Privilege escalation complete',
                '',
                '>>> PAYLOAD DEPLOYED - SYSTEM COMPROMISED'
            ],
            extractor: [
                '>>> DATA EXTRACTION PROTOCOL ACTIVE',
                '>>> SCANNING FILE SYSTEM FOR SENSITIVE DATA',
                '',
                '[SEARCH] Locating password files... 47 found',
                '[SEARCH] Locating database files... 12 found',
                '[SEARCH] Locating configuration files... 156 found',
                '[SEARCH] Locating user documents... 2,847 found',
                '',
                '[EXTRACT] /etc/passwd (1.2 KB)',
                '[EXTRACT] /etc/shadow (892 B)',
                '[EXTRACT] /var/log/auth.log (45.7 KB)',
                '[EXTRACT] /home/admin/documents/financial.xlsx (2.1 MB)',
                '[EXTRACT] /var/www/html/config/database.php (743 B)',
                '',
                '[COMPRESS] Creating encrypted archive...',
                '[UPLOAD] Transferring to secure server...',
                '',
                '>>> DATA EXTRACTION COMPLETE - 48.3 MB ACQUIRED'
            ],
            sniffer: [
                '>>> PACKET SNIFFER ONLINE',
                '>>> MONITORING NETWORK INTERFACE: eth0',
                '',
                '[CAPTURE] TCP 192.168.1.100:80 -> 10.0.0.5:54321 [HTTP GET /login.php]',
                '[CAPTURE] TCP 192.168.1.100:443 -> 10.0.0.12:43210 [HTTPS POST /api/auth]',
                '[CAPTURE] UDP 192.168.1.100:53 -> 8.8.8.8:53 [DNS Query: example.com]',
                '[CAPTURE] TCP 192.168.1.100:22 -> 10.0.0.8:22000 [SSH Key Exchange]',
                '',
                '[ANALYSIS] Detected plaintext credentials in HTTP traffic',
                '[ANALYSIS] Username: admin | Password: SecretPass123',
                '[ANALYSIS] Session token captured: JSESSIONID=A7B9C2D4E6F8',
                '',
                '[INTERCEPT] Hijacking active session...',
                '[SUCCESS] Session takeover complete',
                '',
                '>>> NETWORK TRAFFIC COMPROMISED'
            ]
        };

        // Initialize audio
        function initAudio() {
            if (!audioContext) {
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
            }
        }

        // Create typing/clicking sounds
        function playClickSound() {
            if (!soundEnabled || !audioContext) return;
            
            const oscillator = audioContext.createOscillator();
            const gainNode = audioContext.createGain();
            
            oscillator.connect(gainNode);
            gainNode.connect(audioContext.destination);
            
            oscillator.frequency.setValueAtTime(1200 + Math.random() * 800, audioContext.currentTime);
            oscillator.type = 'square';
            
            gainNode.gain.setValueAtTime(0.05, audioContext.currentTime);
            gainNode.gain.exponentialRampToValueAtTime(0.001, audioContext.currentTime + 0.05);
            
            oscillator.start();
            oscillator.stop(audioContext.currentTime + 0.05);
        }

        function playBeepSound() {
            if (!soundEnabled || !audioContext) return;
            
            const oscillator = audioContext.createOscillator();
            const gainNode = audioContext.createGain();
            
            oscillator.connect(gainNode);
            gainNode.connect(audioContext.destination);
            
            oscillator.frequency.setValueAtTime(2000, audioContext.currentTime);
            oscillator.type = 'sine';
            
            gainNode.gain.setValueAtTime(0.1, audioContext.currentTime);
            gainNode.gain.exponentialRampToValueAtTime(0.001, audioContext.currentTime + 0.3);
            
            oscillator.start();
            oscillator.stop(audioContext.currentTime + 0.3);
        }

        // Matrix rain effect
        function initMatrix() {
            const canvas = document.getElementById('matrixCanvas');
            const ctx = canvas.getContext('2d');
            
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            
            const chars = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ<>{}[]()!@#$%^&*';
            const charArray = chars.split('');
            const fontSize = 12;
            const columns = canvas.width / fontSize;
            const drops = [];
            
            for (let x = 0; x < columns; x++) {
                drops[x] = 1;
            }
            
            function draw() {
                ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
                ctx.fillRect(0, 0, canvas.width, canvas.height);
                
                ctx.fillStyle = '#00ff41';
                ctx.font = fontSize + 'px monospace';
                
                for (let i = 0; i < drops.length; i++) {
                    const text = charArray[Math.floor(Math.random() * charArray.length)];
                    ctx.fillText(text, i * fontSize, drops[i] * fontSize);
                    
                    if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
                        drops[i] = 0;
                    }
                    drops[i]++;
                }
            }
            
            setInterval(draw, 50);
        }

        // Custom cursor
        function initCursor() {
            const cursor = document.getElementById('customCursor');
            
            document.addEventListener('mousemove', (e) => {
                cursor.style.left = e.clientX - 12 + 'px';
                cursor.style.top = e.clientY - 12 + 'px';
            });
        }

        // Type text with animation
        function typeText(element, text, callback) {
            element.innerHTML = '';
            let i = 0;
            
            const typing = setInterval(() => {
                if (Math.random() < 0.3) playClickSound();
                
                element.innerHTML += text[i];
                i++;
                
                if (i >= text.length) {
                    clearInterval(typing);
                    if (callback) setTimeout(callback, 800);
                }
                
                element.scrollTop = element.scrollHeight;
            }, animationSpeed + Math.random() * 50);
        }

        // Activate tool
        function activateTool(toolName) {
            // Update button states
            document.querySelectorAll('.tool-button').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            
            currentTool = toolName;
            playBeepSound();
            
            // Clear and start new output
            const output = document.getElementById('mainOutput');
            output.innerHTML = '';
            
            runToolSequence(toolName);
        }

        function runToolSequence(toolName) {
            const commands = toolOutputs[toolName] || toolOutputs.scanner;
            let index = 0;
            
            function nextCommand() {
                if (index >= commands.length) {
                    setTimeout(() => runToolSequence(toolName), 3000);
                    return;
                }
                
                const output = document.getElementById('mainOutput');
                const line = commands[index];
                
                if (line === '') {
                    output.innerHTML += '<br>';
                    index++;
                    setTimeout(nextCommand, 200);
                    return;
                }
                
                const div = document.createElement('div');
                div.style.marginBottom = '5px';
                
                if (line.includes('[SUCCESS]')) {
                    div.style.color = '#00ff41';
                    div.style.textShadow = '0 0 5px #00ff41';
                }
                else if (line.includes('[FAILED]') || line.includes('[VULN]')) {
                    div.style.color = '#ff4141';
                }
                else if (line.includes('[ATTEMPT]') || line.includes('[SCAN]')) {
                    div.style.color = '#ffff41';
                }
                else if (line.includes('>>>')) {
                    div.style.color = '#00ff41';
                    div.style.fontWeight = 'bold';
                    div.style.textShadow = '0 0 8px #00ff41';
                }
                
                output.appendChild(div);
                typeText(div, line, () => {
                    index++;
                    nextCommand();
                });
            }
            
            nextCommand();
        }

        // Update progress bars
        function updateProgress() {
            const infiltration = document.getElementById('infiltrationProgress');
            const extraction = document.getElementById('extractionProgress');
            const stealth = document.getElementById('stealthProgress');
            
            const infValue = Math.min(100, parseInt(infiltration.style.width) + Math.random() * 3);
            const extValue = Math.min(100, parseInt(extraction.style.width) + Math.random() * 2);
            const stealthValue = Math.max(60, 85 + Math.random() * 10 - 5);
            
            infiltration.style.width = infValue + '%';
            extraction.style.width = extValue + '%';
            stealth.style.width = stealthValue + '%';
        }

        // Update network map
        function updateNetworkMap() {
            const map = document.getElementById('networkMap');
            const nodes = map.querySelectorAll('.node');
            
            // Remove old nodes
            nodes.forEach(node => node.remove());
            
            // Add new nodes
            for (let i = 0; i < 6; i++) {
                const node = document.createElement('div');
                node.className = 'node';
                node.style.left = Math.random() * 90 + '%';
                node.style.top = Math.random() * 80 + 10 + '%';
                node.style.animationDelay = Math.random() * 2 + 's';
                map.appendChild(node);
            }
            
            // Add connection lines
            for (let i = 0; i < 3; i++) {
                const line = document.createElement('div');
                line.className = 'connection-line';
                line.style.left = Math.random() * 80 + '%';
                line.style.top = Math.random() * 70 + 15 + '%';
                line.style.width = Math.random() * 100 + 50 + 'px';
                line.style.animationDelay = Math.random() * 1.5 + 's';
                map.appendChild(line);
            }
        }

        // Update stats
        function updateStats() {
            document.getElementById('targetCount').textContent = Math.floor(Math.random() * 50) + 100;
            document.getElementById('exploitCount').textContent = Math.floor(Math.random() * 20) + 80;
            document.getElementById('dataSize').textContent = (Math.random() * 500 + 100).toFixed(1);
            
            // Update target info
            const ips = ['192.168.1.100', '10.0.0.50', '172.16.1.200', '192.168.0.1'];
            const hostnames = ['MAINFRAME-SRV', 'DB-SERVER-01', 'WEB-PROXY', 'DOMAIN-CTRL'];
            const systems = ['WINDOWS 2019', 'LINUX UBUNTU', 'FREEBSD 13.1', 'WINDOWS 10'];
            
            document.getElementById('targetIP').textContent = ips[Math.floor(Math.random() * ips.length)];
            document.getElementById('hostname').textContent = hostnames[Math.floor(Math.random() * hostnames.length)];
            document.getElementById('targetOS').textContent = systems[Math.floor(Math.random() * systems.length)];
            document.getElementById('firewall').textContent = Math.random() > 0.5 ? 'ACTIVE' : 'BYPASSED';
        }

        // Update system time
        function updateTime() {
            const now = new Date();
            document.getElementById('systemTime').textContent = 
                now.toLocaleTimeString() + ' | ' + now.toLocaleDateString();
        }

        // Toggle sound
        function toggleSound() {
            initAudio();
            soundEnabled = !soundEnabled;
            document.getElementById('soundToggle').textContent = soundEnabled ? '🔊 ASMR ON' : '🔇 ASMR OFF';
            
            if (soundEnabled) {
                playBeepSound();
            }
        }

        // Random glitch effects
        function addGlitchEffect() {
            const elements = document.querySelectorAll('.terminal-content, .target-info, .section-title');
            const randomElement = elements[Math.floor(Math.random() * elements.length)];
            
            randomElement.classList.add('glitch-text');
            setTimeout(() => {
                randomElement.classList.remove('glitch-text');
            }, 300);
        }

        // Update alert messages
        function updateAlerts() {
            const alerts = [
                '⚠️ SECURITY BREACH DETECTED - INITIALIZING COUNTERMEASURES',
                '🚨 FIREWALL BYPASS SUCCESSFUL - STEALTH MODE ACTIVE',
                '⚡ INTRUSION DETECTED - DEPLOYING DEFENSIVE PROTOCOLS',
                '🔥 SYSTEM VULNERABILITY EXPLOITED - GAINING ROOT ACCESS',
                '💀 BACKDOOR INSTALLED - MAINTAINING PERSISTENCE',
                '🎯 TARGET ACQUIRED - BEGINNING DATA HARVEST',
                '👻 GHOST MODE ENABLED - INVISIBLE TO DETECTION'
            ];
            
            const alertBox = document.getElementById('alertBox');
            alertBox.textContent = alerts[Math.floor(Math.random() * alerts.length)];
        }

        // Add random data packets to stream
        function addDataPackets() {
            const output = document.getElementById('mainOutput');
            const packets = [
                '<span class="data-packet">[PKT] 0x7F4A2B8C -> INTERCEPTED</span>',
                '<span class="data-packet">[PKT] 0x9E3D1A5F -> DECRYPTED</span>',
                '<span class="data-packet">[PKT] 0x6B8C4E2A -> ANALYZED</span>',
                '<span class="data-packet">[PKT] 0x1F9A7C3D -> STORED</span>'
            ];
            
            if (Math.random() < 0.3) {
                const packet = packets[Math.floor(Math.random() * packets.length)];
                const div = document.createElement('div');
                div.innerHTML = packet;
                div.style.opacity = '0.6';
                output.appendChild(div);
                output.scrollTop = output.scrollHeight;
            }
        }

        // Keyboard interactions
        document.addEventListener('keydown', (e) => {
            playClickSound();
            
            // Easter eggs for specific keys
            if (e.key === 'Enter') {
                playBeepSound();
                addGlitchEffect();
            }
            
            if (e.code === 'Space') {
                updateNetworkMap();
            }
        });

        // Click sounds for buttons
        document.addEventListener('click', () => {
            playClickSound();
        });

        // Initialize everything
        window.addEventListener('load', () => {
            initMatrix();
            initCursor();
            updateTime();
            updateNetworkMap();
            
            // Start with scanner tool
            runToolSequence('scanner');
            
            // Set up intervals
            setInterval(updateProgress, 2000);
            setInterval(updateStats, 5000);
            setInterval(updateTime, 1000);
            setInterval(updateAlerts, 8000);
            setInterval(addGlitchEffect, 12000);
            setInterval(updateNetworkMap, 15000);
            setInterval(addDataPackets, 1500);
            
            // Initial updates
            updateStats();
            updateAlerts();
        });

        // Handle window resize
        window.addEventListener('resize', () => {
            const canvas = document.getElementById('matrixCanvas');
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });
    </script>
</body>
</html>
