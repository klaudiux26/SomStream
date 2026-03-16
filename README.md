<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SomStream - Streaming de Música</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --bg-primary: #0a0a0a;
            --bg-secondary: #121212;
            --bg-tertiary: #1a1a1a;
            --bg-elevated: #242424;
            --text-primary: #ffffff;
            --text-secondary: #b3b3b3;
            --accent: #1db954;
            --accent-hover: #1ed760;
            --glass: rgba(255, 255, 255, 0.05);
            --glass-border: rgba(255, 255, 255, 0.1);
        }

        body {
            font-family: 'Inter', sans-serif;
            background: var(--bg-primary);
            color: var(--text-primary);
            overflow: hidden;
            height: 100vh;
        }

        /* Layout Principal */
        .app-container {
            display: grid;
            grid-template-columns: 280px 1fr 300px;
            grid-template-rows: 1fr 90px;
            height: 100vh;
            gap: 8px;
            padding: 8px;
            background: var(--bg-primary);
        }

        /* Scrollbar Customizada */
        ::-webkit-scrollbar {
            width: 12px;
        }

        ::-webkit-scrollbar-track {
            background: transparent;
        }

        ::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 6px;
            border: 3px solid var(--bg-secondary);
        }

        ::-webkit-scrollbar-thumb:hover {
            background: rgba(255, 255, 255, 0.3);
        }

        /* Sidebar Esquerda - Navegação */
        .sidebar-left {
            background: var(--bg-secondary);
            border-radius: 12px;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            grid-row: 1;
        }

        .nav-section {
            padding: 24px 16px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 12px;
            font-size: 24px;
            font-weight: 800;
            margin-bottom: 24px;
            padding: 0 8px;
            background: linear-gradient(135deg, #1db954 0%, #1ed760 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .logo i {
            font-size: 32px;
            -webkit-text-fill-color: #1db954;
        }

        .nav-item {
            display: flex;
            align-items: center;
            gap: 16px;
            padding: 12px 16px;
            margin: 4px 0;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            color: var(--text-secondary);
            font-weight: 600;
            font-size: 14px;
        }

        .nav-item:hover {
            background: var(--glass);
            color: var(--text-primary);
            transform: translateX(4px);
        }

        .nav-item.active {
            background: var(--glass);
            color: var(--text-primary);
        }

        .nav-item i {
            font-size: 20px;
            width: 24px;
            text-align: center;
        }

        /* Biblioteca */
        .library-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 16px;
            cursor: pointer;
        }

        .library-title {
            display: flex;
            align-items: center;
            gap: 12px;
            color: var(--text-secondary);
            font-weight: 700;
            font-size: 14px;
            transition: color 0.3s;
        }

        .library-title:hover {
            color: var(--text-primary);
        }

        .library-actions {
            display: flex;
            gap: 8px;
        }

        .icon-btn {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            border: none;
            background: transparent;
            color: var(--text-secondary);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }

        .icon-btn:hover {
            background: var(--glass);
            color: var(--text-primary);
            transform: scale(1.1);
        }

        .playlist-list {
            flex: 1;
            overflow-y: auto;
            padding: 0 8px;
        }

        .playlist-item {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 8px 12px;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.2s;
            margin: 2px 0;
        }

        .playlist-item:hover {
            background: var(--glass);
        }

        .playlist-cover {
            width: 48px;
            height: 48px;
            border-radius: 4px;
            background: linear-gradient(135deg, #333 0%, #555 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            color: var(--text-secondary);
            position: relative;
            overflow: hidden;
        }

        .playlist-cover img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .playlist-info {
            flex: 1;
            min-width: 0;
        }

        .playlist-name {
            font-size: 14px;
            font-weight: 600;
            color: var(--text-primary);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .playlist-meta {
            font-size: 12px;
            color: var(--text-secondary);
            margin-top: 2px;
        }

        /* Conteúdo Principal */
        .main-content {
            background: linear-gradient(180deg, #1a1a2e 0%, var(--bg-secondary) 100%);
            border-radius: 12px;
            overflow-y: auto;
            position: relative;
            grid-row: 1;
        }

        .top-bar {
            position: sticky;
            top: 0;
            padding: 16px 32px;
            background: rgba(18, 18, 18, 0.8);
            backdrop-filter: blur(20px);
            display: flex;
            align-items: center;
            justify-content: space-between;
            z-index: 100;
            border-bottom: 1px solid var(--glass-border);
        }

        .nav-buttons {
            display: flex;
            gap: 12px;
        }

        .nav-btn {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            border: none;
            background: rgba(0, 0, 0, 0.5);
            color: var(--text-primary);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }

        .nav-btn:hover {
            background: rgba(0, 0, 0, 0.8);
            transform: scale(1.1);
        }

        .search-box {
            flex: 1;
            max-width: 400px;
            margin: 0 24px;
            position: relative;
        }

        .search-input {
            width: 100%;
            padding: 10px 16px 10px 40px;
            border-radius: 24px;
            border: none;
            background: rgba(255, 255, 255, 0.1);
            color: var(--text-primary);
            font-size: 14px;
            outline: none;
            transition: all 0.3s;
        }

        .search-input:focus {
            background: rgba(255, 255, 255, 0.15);
            box-shadow: 0 0 0 2px rgba(255, 255, 255, 0.1);
        }

        .search-box i {
            position: absolute;
            left: 14px;
            top: 50%;
            transform: translateY(-50%);
            color: var(--text-secondary);
            font-size: 14px;
        }

        .user-menu {
            display: flex;
            align-items: center;
            gap: 16px;
        }

        .upgrade-btn {
            padding: 8px 20px;
            border-radius: 20px;
            border: 1px solid rgba(255, 255, 255, 0.3);
            background: transparent;
            color: var(--text-primary);
            font-weight: 600;
            font-size: 12px;
            cursor: pointer;
            transition: all 0.3s;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .upgrade-btn:hover {
            transform: scale(1.05);
            border-color: var(--text-primary);
            background: rgba(255, 255, 255, 0.05);
        }

        .user-avatar {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            cursor: pointer;
            transition: transform 0.3s;
            position: relative;
        }

        .user-avatar:hover {
            transform: scale(1.1);
        }

        .user-avatar::after {
            content: '';
            position: absolute;
            inset: -2px;
            border-radius: 50%;
            border: 2px solid transparent;
            background: linear-gradient(135deg, #1db954, #1ed760) border-box;
            -webkit-mask: linear-gradient(#fff 0 0) padding-box, linear-gradient(#fff 0 0);
            mask: linear-gradient(#fff 0 0) padding-box, linear-gradient(#fff 0 0);
            -webkit-mask-composite: xor;
            mask-composite: exclude;
        }

        /* Conteúdo */
        .content {
            padding: 24px 32px;
        }

        .section-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 24px;
        }

        .section-title {
            font-size: 28px;
            font-weight: 800;
            letter-spacing: -0.5px;
        }

        .show-all {
            color: var(--text-secondary);
            font-size: 12px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 1px;
            cursor: pointer;
            transition: color 0.3s;
        }

        .show-all:hover {
            color: var(--text-primary);
            text-decoration: underline;
        }

        .cards-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 24px;
            margin-bottom: 40px;
        }

        .card {
            background: rgba(255, 255, 255, 0.03);
            padding: 16px;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            group: card;
        }

        .card:hover {
            background: rgba(255, 255, 255, 0.08);
            transform: translateY(-4px);
        }

        .card-image {
            width: 100%;
            aspect-ratio: 1;
            border-radius: 6px;
            overflow: hidden;
            position: relative;
            margin-bottom: 16px;
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.4);
        }

        .card-image img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.4s;
        }

        .card:hover .card-image img {
            transform: scale(1.05);
        }

        .play-button {
            position: absolute;
            bottom: 8px;
            right: 8px;
            width: 48px;
            height: 48px;
            border-radius: 50%;
            background: var(--accent);
            border: none;
            color: #000;
            font-size: 18px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            opacity: 0;
            transform: translateY(8px);
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
        }

        .card:hover .play-button {
            opacity: 1;
            transform: translateY(0);
        }

        .play-button:hover {
            transform: scale(1.1);
            background: var(--accent-hover);
        }

        .card-title {
            font-size: 15px;
            font-weight: 700;
            margin-bottom: 6px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .card-description {
            font-size: 13px;
            color: var(--text-secondary);
            line-height: 1.4;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            overflow: hidden;
        }

        /* Sidebar Direita - Fila de Reprodução */
        .sidebar-right {
            background: var(--bg-secondary);
            border-radius: 12px;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            grid-row: 1;
        }

        .now-playing-header {
            padding: 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
        }

        .now-playing-title {
            font-size: 16px;
            font-weight: 700;
            margin-bottom: 4px;
        }

        .now-playing-subtitle {
            font-size: 12px;
            color: var(--text-secondary);
        }

        .now-playing-art {
            width: 100%;
            aspect-ratio: 1;
            padding: 20px;
        }

        .now-playing-art img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            border-radius: 8px;
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.6);
        }

        .now-playing-info {
            padding: 0 20px 20px;
        }

        .track-name {
            font-size: 20px;
            font-weight: 700;
            margin-bottom: 4px;
            cursor: pointer;
        }

        .track-name:hover {
            text-decoration: underline;
        }

        .artist-name {
            font-size: 14px;
            color: var(--text-secondary);
            cursor: pointer;
        }

        .artist-name:hover {
            color: var(--text-primary);
            text-decoration: underline;
        }

        .queue-section {
            flex: 1;
            overflow-y: auto;
            padding: 16px;
        }

        .queue-title {
            font-size: 14px;
            font-weight: 700;
            margin-bottom: 16px;
            color: var(--text-secondary);
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .queue-item {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 8px;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.2s;
            margin-bottom: 4px;
        }

        .queue-item:hover {
            background: var(--glass);
        }

        .queue-item.playing {
            background: rgba(29, 185, 84, 0.1);
        }

        .queue-number {
            width: 20px;
            text-align: center;
            font-size: 12px;
            color: var(--text-secondary);
            font-variant-numeric: tabular-nums;
        }

        .queue-item.playing .queue-number {
            color: var(--accent);
        }

        .queue-img {
            width: 40px;
            height: 40px;
            border-radius: 4px;
            object-fit: cover;
        }

        .queue-info {
            flex: 1;
            min-width: 0;
        }

        .queue-track {
            font-size: 13px;
            font-weight: 600;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .queue-item.playing .queue-track {
            color: var(--accent);
        }

        .queue-artist {
            font-size: 11px;
            color: var(--text-secondary);
        }

        /* Player Fixo */
        .player {
            grid-column: 1 / -1;
            background: rgba(18, 18, 18, 0.95);
            backdrop-filter: blur(20px);
            border-top: 1px solid rgba(255, 255, 255, 0.05);
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 24px;
            position: relative;
            z-index: 1000;
        }

        .player-left {
            display: flex;
            align-items: center;
            gap: 16px;
            width: 30%;
            min-width: 200px;
        }

        .player-cover {
            width: 56px;
            height: 56px;
            border-radius: 4px;
            overflow: hidden;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
            position: relative;
        }

        .player-cover img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .player-track-info {
            display: flex;
            flex-direction: column;
            gap: 4px;
        }

        .player-track-name {
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
        }

        .player-track-name:hover {
            text-decoration: underline;
        }

        .player-artist {
            font-size: 11px;
            color: var(--text-secondary);
            cursor: pointer;
        }

        .player-artist:hover {
            color: var(--text-primary);
            text-decoration: underline;
        }

        .like-btn {
            margin-left: 16px;
            color: var(--text-secondary);
            cursor: pointer;
            transition: all 0.3s;
            font-size: 18px;
        }

        .like-btn:hover {
            color: var(--accent);
            transform: scale(1.2);
        }

        .like-btn.active {
            color: var(--accent);
        }

        .player-center {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            flex: 1;
            max-width: 600px;
        }

        .controls {
            display: flex;
            align-items: center;
            gap: 24px;
        }

        .control-btn {
            background: none;
            border: none;
            color: var(--text-secondary);
            font-size: 20px;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 32px;
            height: 32px;
        }

        .control-btn:hover {
            color: var(--text-primary);
            transform: scale(1.1);
        }

        .play-pause {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: var(--text-primary);
            color: #000;
            font-size: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }

        .play-pause:hover {
            transform: scale(1.1);
            background: var(--accent);
        }

        .progress-container {
            display: flex;
            align-items: center;
            gap: 12px;
            width: 100%;
            max-width: 600px;
        }

        .time {
            font-size: 11px;
            color: var(--text-secondary);
            font-variant-numeric: tabular-nums;
            min-width: 35px;
            text-align: center;
        }

        .progress-bar {
            flex: 1;
            height: 4px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 2px;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: var(--text-primary);
            border-radius: 2px;
            width: 35%;
            position: relative;
            transition: width 0.1s;
        }

        .progress-fill::after {
            content: '';
            position: absolute;
            right: -6px;
            top: 50%;
            transform: translateY(-50%);
            width: 12px;
            height: 12px;
            background: var(--text-primary);
            border-radius: 50%;
            opacity: 0;
            transition: opacity 0.3s;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }

        .progress-bar:hover .progress-fill::after {
            opacity: 1;
        }

        .progress-bar:hover .progress-fill {
            background: var(--accent);
        }

        .player-right {
            display: flex;
            align-items: center;
            gap: 16px;
            width: 30%;
            justify-content: flex-end;
            min-width: 200px;
        }

        .volume-container {
            display: flex;
            align-items: center;
            gap: 8px;
            width: 150px;
        }

        .volume-slider {
            flex: 1;
            height: 4px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 2px;
            cursor: pointer;
            position: relative;
        }

        .volume-fill {
            height: 100%;
            background: var(--text-primary);
            border-radius: 2px;
            width: 70%;
            transition: width 0.1s;
        }

        .volume-slider:hover .volume-fill {
            background: var(--accent);
        }

        /* Animações */
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        .playing-indicator {
            display: flex;
            gap: 2px;
            align-items: flex-end;
            height: 12px;
        }

        .bar {
            width: 3px;
            background: var(--accent);
            animation: equalizer 1s ease-in-out infinite;
            border-radius: 1px;
        }

        .bar:nth-child(1) { height: 40%; animation-delay: 0s; }
        .bar:nth-child(2) { height: 80%; animation-delay: 0.2s; }
        .bar:nth-child(3) { height: 60%; animation-delay: 0.4s; }

        @keyframes equalizer {
            0%, 100% { transform: scaleY(1); }
            50% { transform: scaleY(1.5); }
        }

        /* Responsividade */
        @media (max-width: 1200px) {
            .app-container {
                grid-template-columns: 240px 1fr;
            }
            .sidebar-right {
                display: none;
            }
        }

        @media (max-width: 768px) {
            .app-container {
                grid-template-columns: 1fr;
                grid-template-rows: 1fr 90px;
            }
            .sidebar-left {
                display: none;
            }
            .player-left, .player-right {
                min-width: auto;
            }
            .player-cover {
                width: 40px;
                height: 40px;
            }
            .cards-grid {
                grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
                gap: 16px;
            }
        }

        /* Glassmorphism extra */
        .glass-panel {
            background: var(--glass);
            backdrop-filter: blur(20px);
            border: 1px solid var(--glass-border);
            border-radius: 12px;
        }

        /* Efeito de gradiente animado no fundo */
        .gradient-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
            opacity: 0.4;
            background: 
                radial-gradient(circle at 20% 50%, rgba(120, 119, 198, 0.3) 0%, transparent 50%),
                radial-gradient(circle at 80% 80%, rgba(29, 185, 84, 0.15) 0%, transparent 50%);
            filter: blur(60px);
            animation: gradientShift 20s ease infinite;
        }

        @keyframes gradientShift {
            0%, 100% { transform: translate(0, 0) scale(1); }
            33% { transform: translate(30px, -30px) scale(1.1); }
            66% { transform: translate(-20px, 20px) scale(0.9); }
        }
    </style>
</head>
<body>
    <div class="gradient-bg"></div>
    
    <div class="app-container">
        <!-- Sidebar Esquerda -->
        <aside class="sidebar-left">
            <div class="nav-section">
                <div class="logo">
                    <i class="fas fa-wave-square"></i>
                    <span>SomStream</span>
                </div>
                
                <nav>
                    <div class="nav-item active">
                        <i class="fas fa-home"></i>
                        <span>Início</span>
                    </div>
                    <div class="nav-item">
                        <i class="fas fa-search"></i>
                        <span>Buscar</span>
                    </div>
                </nav>
            </div>

            <div class="library-section">
                <div class="library-header">
                    <div class="library-title">
                        <i class="fas fa-book"></i>
                        <span>Sua Biblioteca</span>
                    </div>
                    <div class="library-actions">
                        <button class="icon-btn" title="Criar playlist">
                            <i class="fas fa-plus"></i>
                        </button>
                        <button class="icon-btn" title="Expandir">
                            <i class="fas fa-arrow-right"></i>
                        </button>
                    </div>
                </div>

                <div class="playlist-list">
                    <div class="playlist-item">
                        <div class="playlist-cover" style="background: linear-gradient(135deg, #450af5, #c4efd9);">
                            <i class="fas fa-heart"></i>
                        </div>
                        <div class="playlist-info">
                            <div class="playlist-name">Músicas Curtidas</div>
                            <div class="playlist-meta">Playlist • 847 músicas</div>
                        </div>
                    </div>

                    <div class="playlist-item">
                        <div class="playlist-cover">
                            <img src="https://images.unsplash.com/photo-1493225255756-d9584f8606e9?w=100&h=100&fit=crop" alt="Capa">
                        </div>
                        <div class="playlist-info">
                            <div class="playlist-name">Descobertas da Semana</div>
                            <div class="playlist-meta">Playlist • 30 músicas</div>
                        </div>
                    </div>

                    <div class="playlist-item">
                        <div class="playlist-cover">
                            <img src="https://images.unsplash.com/photo-1514525253161-7a46d19cd819?w=100&h=100&fit=crop" alt="Capa">
                        </div>
                        <div class="playlist-info">
                            <div class="playlist-name">Mix de Rock</div>
                            <div class="playlist-meta">Playlist • 50 músicas</div>
                        </div>
                    </div>

                    <div class="playlist-item">
                        <div class="playlist-cover">
                            <img src="https://images.unsplash.com/photo-1511671782779-c97d3d27a1d4?w=100&h=100&fit=crop" alt="Capa">
                        </div>
                        <div class="playlist-info">
                            <div class="playlist-name">MPB Brasil</div>
                            <div class="playlist-meta">Playlist • 120 músicas</div>
                        </div>
                    </div>

                    <div class="playlist-item">
                        <div class="playlist-cover">
                            <img src="https://images.unsplash.com/photo-1470225620780-dba8ba36b745?w=100&h=100&fit=crop" alt="Capa">
                        </div>
                        <div class="playlist-info">
                            <div class="playlist-name">Eletrônicas 2024</div>
                            <div class="playlist-meta">Playlist • 85 músicas</div>
                        </div>
                    </div>

                    <div class="playlist-item">
                        <div class="playlist-cover" style="background: linear-gradient(135deg, #ff6b6b, #feca57);">
                            <i class="fas fa-clock"></i>
                        </div>
                        <div class="playlist-info">
                            <div class="playlist-name">Ouvidas Recentemente</div>
                            <div class="playlist-meta">Playlist • 45 músicas</div>
                        </div>
                    </div>
                </div>
            </div>
        </aside>

        <!-- Conteúdo Principal -->
        <main class="main-content">
            <div class="top-bar">
                <div class="nav-buttons">
                    <button class="nav-btn">
                        <i class="fas fa-chevron-left"></i>
                    </button>
                    <button class="nav-btn">
                        <i class="fas fa-chevron-right"></i>
                    </button>
                </div>

                <div class="search-box">
                    <i class="fas fa-search"></i>
                    <input type="text" class="search-input" placeholder="O que você quer ouvir?">
                </div>

                <div class="user-menu">
                    <button class="upgrade-btn">Premium</button>
                    <div class="user-avatar">JS</div>
                </div>
            </div>

            <div class="content">
                <section>
                    <div class="section-header">
                        <h2 class="section-title">Boa tarde</h2>
                        <span class="show-all">Mostrar tudo</span>
                    </div>
                    
                    <div class="cards-grid">
                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1614613535308-eb5fbd3d2c17?w=300&h=300&fit=crop" alt="Mix Diário">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">Mix Diário 1</div>
                            <div class="card-description">The Weeknd, Dua Lipa, Doja Cat e mais</div>
                        </div>

                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1493225255756-d9584f8606e9?w=300&h=300&fit=crop" alt="Descobertas">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">Descobertas da Semana</div>
                            <div class="card-description">Novas músicas escolhidas especialmente para você</div>
                        </div>

                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1514525253161-7a46d19cd819?w=300&h=300&fit=crop" alt="Rock">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">Mix de Rock</div>
                            <div class="card-description">Foo Fighters, Red Hot Chili Peppers, Nirvana</div>
                        </div>

                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1511671782779-c97d3d27a1d4?w=300&h=300&fit=crop" alt="MPB">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">MPB Relax</div>
                            <div class="card-description">Gilberto Gil, Caetano Veloso, Gal Costa</div>
                        </div>

                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1470225620780-dba8ba36b745?w=300&h=300&fit=crop" alt="Eletrônica">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">Eletrônica Focus</div>
                            <div class="card-description">Calvin Harris, David Guetta, Avicii</div>
                        </div>
                    </div>
                </section>

                <section>
                    <div class="section-header">
                        <h2 class="section-title">Feito para você</h2>
                        <span class="show-all">Mostrar tudo</span>
                    </div>
                    
                    <div class="cards-grid">
                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1459749411177-0473ef716175?w=300&h=300&fit=crop" alt="Playlist">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">Radar de Novidades</div>
                            <div class="card-description">As novidades que você não pode perder</div>
                        </div>

                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1508700115892-45ecd05ae2ad?w=300&h=300&fit=crop" alt="Playlist">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">On Repeat</div>
                            <div class="card-description">As músicas que você mais ouviu recentemente</div>
                        </div>

                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1511379938547-c1f69419868d?w=300&h=300&fit=crop" alt="Playlist">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">Descobertas 2024</div>
                            <div class="card-description">Os melhores lançamentos do ano</div>
                        </div>

                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1483412033650-1015ddeb83d1?w=300&h=300&fit=crop" alt="Playlist">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">Acústico</div>
                            <div class="card-description">Versões acústicas das suas favoritas</div>
                        </div>
                    </div>
                </section>

                <section>
                    <div class="section-header">
                        <h2 class="section-title">Seus mixes mais ouvidos</h2>
                        <span class="show-all">Mostrar tudo</span>
                    </div>
                    
                    <div class="cards-grid">
                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1501612780327-45045538702b?w=300&h=300&fit=crop" alt="Mix">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">Pop Mix</div>
                            <div class="card-description">Taylor Swift, Olivia Rodrigo, Harry Styles</div>
                        </div>

                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1511735111819-9a3f77ebd235?w=300&h=300&fit=crop" alt="Mix">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">Hip Hop Mix</div>
                            <div class="card-description">Drake, Kendrick Lamar, Travis Scott</div>
                        </div>

                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1484876065684-b683cf17d276?w=300&h=300&fit=crop" alt="Mix">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">Indie Mix</div>
                            <div class="card-description">Arctic Monkeys, Tame Impala, MGMT</div>
                        </div>

                        <div class="card">
                            <div class="card-image">
                                <img src="https://images.unsplash.com/photo-1415201364774-f6f0bb35f28f?w=300&h=300&fit=crop" alt="Mix">
                                <button class="play-button">
                                    <i class="fas fa-play"></i>
                                </button>
                            </div>
                            <div class="card-title">Jazz Mix</div>
                            <div class="card-description">Miles Davis, John Coltrane, Billie Holiday</div>
                        </div>
                    </div>
                </section>
            </div>
        </main>

        <!-- Sidebar Direita - Fila -->
        <aside class="sidebar-right">
            <div class="now-playing-header">
                <div class="now-playing-title">Tocando Agora</div>
                <div class="now-playing-subtitle">Fila de reprodução</div>
            </div>

            <div class="now-playing-art">
                <img src="https://images.unsplash.com/photo-1614613535308-eb5fbd3d2c17?w=400&h=400&fit=crop" alt="Album Art">
            </div>

            <div class="now-playing-info">
                <div class="track-name">Blinding Lights</div>
                <div class="artist-name">The Weeknd</div>
            </div>

            <div class="queue-section">
                <div class="queue-title">Próximas</div>
                
                <div class="queue-item playing">
                    <div class="playing-indicator">
                        <div class="bar"></div>
                        <div class="bar"></div>
                        <div class="bar"></div>
                    </div>
                    <img src="https://images.unsplash.com/photo-1614613535308-eb5fbd3d2c17?w=100&h=100&fit=crop" class="queue-img" alt="">
                    <div class="queue-info">
                        <div class="queue-track">Blinding Lights</div>
                        <div class="queue-artist">The Weeknd</div>
                    </div>
                </div>

                <div class="queue-item">
                    <span class="queue-number">1</span>
                    <img src="https://images.unsplash.com/photo-1493225255756-d9584f8606e9?w=100&h=100&fit=crop" class="queue-img" alt="">
                    <div class="queue-info">
                        <div class="queue-track">Levitating</div>
                        <div class="queue-artist">Dua Lipa</div>
                    </div>
                </div>

                <div class="queue-item">
                    <span class="queue-number">2</span>
                    <img src="https://images.unsplash.com/photo-1514525253161-7a46d19cd819?w=100&h=100&fit=crop" class="queue-img" alt="">
                    <div class="queue-info">
                        <div class="queue-track">Everlong</div>
                        <div class="queue-artist">Foo Fighters</div>
                    </div>
                </div>

                <div class="queue-item">
                    <span class="queue-number">3</span>
                    <img src="https://images.unsplash.com/photo-1511671782779-c97d3d27a1d4?w=100&h=100&fit=crop" class="queue-img" alt="">
                    <div class="queue-info">
                        <div class="queue-track">Trem das Cores</div>
                        <div class="queue-artist">Caetano Veloso</div>
                    </div>
                </div>

                <div class="queue-item">
                    <span class="queue-number">4</span>
                    <img src="https://images.unsplash.com/photo-1470225620780-dba8ba36b745?w=100&h=100&fit=crop" class="queue-img" alt="">
                    <div class="queue-info">
                        <div class="queue-track">Summer</div>
                        <div class="queue-artist">Calvin Harris</div>
                    </div>
                </div>

                <div class="queue-item">
                    <span class="queue-number">5</span>
                    <img src="https://images.unsplash.com/photo-1501612780327-45045538702b?w=100&h=100&fit=crop" class="queue-img" alt="">
                    <div class="queue-info">
                        <div class="queue-track">Anti-Hero</div>
                        <div class="queue-artist">Taylor Swift</div>
                    </div>
                </div>
            </div>
        </aside>

        <!-- Player Fixo -->
        <footer class="player">
            <div class="player-left">
                <div class="player-cover">
                    <img src="https://images.unsplash.com/photo-1614613535308-eb5fbd3d2c17?w=100&h=100&fit=crop" alt="Cover">
                </div>
                <div class="player-track-info">
                    <div class="player-track-name">Blinding Lights</div>
                    <div class="player-artist">The Weeknd</div>
                </div>
                <i class="far fa-heart like-btn"></i>
            </div>

            <div class="player-center">
                <div class="controls">
                    <button class="control-btn" title="Aleatório">
                        <i class="fas fa-random"></i>
                    </button>
                    <button class="control-btn" title="Anterior">
                        <i class="fas fa-step-backward"></i>
                    </button>
                    <button class="control-btn play-pause" title="Play/Pause">
                        <i class="fas fa-pause"></i>
                    </button>
                    <button class="control-btn" title="Próxima">
                        <i class="fas fa-step-forward"></i>
                    </button>
                    <button class="control-btn" title="Repetir">
                        <i class="fas fa-redo"></i>
                    </button>
                </div>
                <div class="progress-container">
                    <span class="time">1:24</span>
                    <div class="progress-bar">
                        <div class="progress-fill"></div>
                    </div>
                    <span class="time">3:20</span>
                </div>
            </div>

            <div class="player-right">
                <button class="control-btn" title="Letras">
                    <i class="fas fa-microphone"></i>
                </button>
                <button class="control-btn" title="Fila">
                    <i class="fas fa-list"></i>
                </button>
                <button class="control-btn" title="Conectar dispositivo">
                    <i class="fas fa-desktop"></i>
                </button>
                <div class="volume-container">
                    <button class="control-btn" title="Volume">
                        <i class="fas fa-volume-up"></i>
                    </button>
                    <div class="volume-slider">
                        <div class="volume-fill"></div>
                    </div>
                </div>
                <button class="control-btn" title="Tela cheia">
                    <i class="fas fa-expand"></i>
                </button>
            </div>
        </footer>
    </div>

    <script>
        // Interatividade básica
        document.addEventListener('DOMContentLoaded', () => {
            // Play/Pause toggle
            const playBtn = document.querySelector('.play-pause');
            let isPlaying = true;
            
            playBtn.addEventListener('click', () => {
                isPlaying = !isPlaying;
                playBtn.innerHTML = isPlaying ? '<i class="fas fa-pause"></i>' : '<i class="fas fa-play"></i>';
            });

            // Like button toggle
            const likeBtn = document.querySelector('.like-btn');
            likeBtn.addEventListener('click', () => {
                likeBtn.classList.toggle('active');
                likeBtn.classList.toggle('far');
                likeBtn.classList.toggle('fas');
            });

            // Progress bar interação
            const progressBar = document.querySelector('.progress-bar');
            const progressFill = document.querySelector('.progress-fill');
            
            progressBar.addEventListener('click', (e) => {
                const rect = progressBar.getBoundingClientRect();
                const percent = (e.clientX - rect.left) / rect.width;
                progressFill.style.width = `${percent * 100}%`;
            });

            // Volume interação
            const volumeSlider = document.querySelector('.volume-slider');
            const volumeFill = document.querySelector('.volume-fill');
            
            volumeSlider.addEventListener('click', (e) => {
                const rect = volumeSlider.getBoundingClientRect();
                const percent = (e.clientX - rect.left) / rect.width;
                volumeFill.style.width = `${percent * 100}%`;
            });

            // Navegação items
            const navItems = document.querySelectorAll('.nav-item');
            navItems.forEach(item => {
                item.addEventListener('click', () => {
                    navItems.forEach(i => i.classList.remove('active'));
                    item.classList.add('active');
                });
            });

            // Cards hover effect com delay
            const cards = document.querySelectorAll('.card');
            cards.forEach((card, index) => {
                card.style.animationDelay = `${index * 0.05}s`;
            });

            // Simulação de progresso da música
            let progress = 35;
            setInterval(() => {
                if (isPlaying && progress < 100) {
                    progress += 0.1;
                    progressFill.style.width = `${progress}%`;
                } else if (progress >= 100) {
                    progress = 0;
                }
            }, 1000);
        });
    </script>
</body>
</html>
