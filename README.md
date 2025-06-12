<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LifeTalk Academy - Interactive Stoic Experience</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1a1d29 0%, #2d3748 50%, #1a202c 100%);
            color: #f7fafc;
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        .header {
            background: rgba(0,0,0,0.4);
            backdrop-filter: blur(20px);
            padding: 15px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px solid rgba(255,215,0,0.3);
            position: sticky;
            top: 0;
            z-index: 100;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 15px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .logo:hover {
            transform: scale(1.05);
        }
        
        .logo-symbol {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: linear-gradient(45deg, #d4af37, #ffd700);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5em;
            color: #1a202c;
            font-weight: bold;
            animation: pulse 2s infinite;
        }
        
        .status {
            display: flex;
            align-items: center;
            gap: 20px;
        }
        
        .live-indicator {
            display: flex;
            align-items: center;
            gap: 8px;
            background: rgba(16, 185, 129, 0.2);
            padding: 8px 15px;
            border-radius: 20px;
            border: 1px solid #10b981;
        }
        
        .live-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: #10b981;
            animation: blink 1s infinite;
        }
        
        .container {
            max-width: 1200px;
            margin: 30px auto;
            padding: 0 20px;
        }
        
        .scenario-selector {
            background: rgba(255,255,255,0.05);
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 30px;
            border: 1px solid rgba(255,215,0,0.3);
        }
        
        .scenario-title {
            text-align: center;
            font-size: 2em;
            color: #ffd700;
            margin-bottom: 20px;
        }
        
        .scenario-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
        }
        
        .scenario-card {
            background: rgba(0,0,0,0.3);
            border-radius: 15px;
            padding: 25px;
            cursor: pointer;
            transition: all 0.3s;
            border: 2px solid transparent;
            position: relative;
            overflow: hidden;
        }
        
        .scenario-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,215,0,0.1), transparent);
            transition: left 0.5s;
        }
        
        .scenario-card:hover::before {
            left: 100%;
        }
        
        .scenario-card:hover {
            border-color: #ffd700;
            transform: translateY(-5px);
            box-shadow: 0 10px 30px rgba(255,215,0,0.2);
        }
        
        .scenario-card.selected {
            border-color: #10b981;
            background: rgba(16, 185, 129, 0.1);
        }
        
        .scenario-emoji {
            font-size: 2.5em;
            text-align: center;
            margin-bottom: 15px;
        }
        
        .scenario-name {
            font-size: 1.3em;
            font-weight: bold;
            text-align: center;
            margin-bottom: 10px;
            color: #ffd700;
        }
        
        .scenario-description {
            text-align: center;
            opacity: 0.8;
            margin-bottom: 15px;
        }
        
        .difficulty-badge {
            background: rgba(239, 68, 68, 0.3);
            color: #fca5a5;
            padding: 5px 12px;
            border-radius: 12px;
            font-size: 0.8em;
            text-align: center;
        }
        
        .difficulty-medium { background: rgba(255, 187, 36, 0.3); color: #fbbf24; }
        .difficulty-hard { background: rgba(239, 68, 68, 0.3); color: #ef4444; }
        .difficulty-expert { background: rgba(139, 92, 246, 0.3); color: #8b5cf6; }
        
        .conversation-area {
            background: rgba(0,0,0,0.4);
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 30px;
            min-height: 600px;
            display: none;
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        .conversation-area.active {
            display: block;
            animation: slideInUp 0.5s ease-out;
        }
        
        .conversation-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        
        .character-info {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .character-avatar {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5em;
            font-weight: bold;
            background: linear-gradient(45deg, #ef4444, #dc2626);
            animation: avatarPulse 3s infinite;
        }
        
        .character-details h3 {
            color: #ffd700;
            margin-bottom: 5px;
        }
        
        .character-mood {
            font-size: 0.9em;
            opacity: 0.8;
        }
        
        .stress-meter {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .stress-bar {
            width: 100px;
            height: 8px;
            background: rgba(255,255,255,0.1);
            border-radius: 4px;
            overflow: hidden;
        }
        
        .stress-fill {
            height: 100%;
            background: linear-gradient(90deg, #10b981, #fbbf24, #ef4444);
            width: 75%;
            border-radius: 4px;
            transition: width 0.3s;
        }
        
        .conversation-feed {
            max-height: 400px;
            overflow-y: auto;
            margin-bottom: 30px;
            padding: 20px;
            background: rgba(255,255,255,0.02);
            border-radius: 15px;
        }
        
        .message {
            margin-bottom: 20px;
            animation: messageAppear 0.5s ease-out;
        }
        
        .message-npc {
            display: flex;
            align-items: flex-start;
            gap: 15px;
        }
        
        .message-player {
            display: flex;
            align-items: flex-start;
            gap: 15px;
            flex-direction: row-reverse;
            margin-left: 50px;
        }
        
        .message-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            flex-shrink: 0;
        }
        
        .npc-avatar {
            background: linear-gradient(45deg, #ef4444, #dc2626);
        }
        
        .player-avatar {
            background: linear-gradient(45deg, #3b82f6, #1d4ed8);
        }
        
        .message-content {
            background: rgba(255,255,255,0.1);
            padding: 12px 16px;
            border-radius: 15px;
            max-width: 70%;
            position: relative;
        }
        
        .message-npc .message-content {
            border-bottom-left-radius: 5px;
        }
        
        .message-player .message-content {
            border-bottom-right-radius: 5px;
            background: rgba(59, 130, 246, 0.2);
        }
        
        .message-timestamp {
            font-size: 0.7em;
            opacity: 0.5;
            margin-top: 5px;
        }
        
        .stoic-analysis {
            background: rgba(212,175,55,0.1);
            border-left: 3px solid #d4af37;
            padding: 10px 15px;
            margin-top: 10px;
            border-radius: 5px;
            font-style: italic;
            font-size: 0.9em;
        }
        
        .response-options {
            display: grid;
            gap: 15px;
        }
        
        .response-option {
            background: rgba(255,255,255,0.05);
            border: 2px solid rgba(255,255,255,0.1);
            padding: 20px;
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
        }
        
        .response-option::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.1), transparent);
            transition: left 0.5s;
        }
        
        .response-option:hover::before {
            left: 100%;
        }
        
        .response-option:hover {
            border-color: #ffd700;
            transform: translateX(5px);
            background: rgba(255,215,0,0.05);
        }
        
        .response-option.stoic {
            border-color: #d4af37;
            background: rgba(212,175,55,0.1);
        }
        
        .response-option.impulsive {
            border-color: #ef4444;
            background: rgba(239,68,68,0.05);
        }
        
        .response-option.diplomatic {
            border-color: #3b82f6;
            background: rgba(59,130,246,0.05);
        }
        
        .response-text {
            font-size: 1.1em;
            margin-bottom: 10px;
            line-height: 1.4;
        }
        
        .response-meta {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 0.8em;
            opacity: 0.8;
        }
        
        .response-type {
            padding: 3px 8px;
            border-radius: 10px;
            font-weight: bold;
        }
        
        .type-stoic { background: rgba(212,175,55,0.3); color: #d4af37; }
        .type-impulsive { background: rgba(239,68,68,0.3); color: #ef4444; }
        .type-diplomatic { background: rgba(59,130,246,0.3); color: #3b82f6; }
        
        .virtue-impact {
            display: flex;
            gap: 5px;
        }
        
        .virtue-meter {
            width: 20px;
            height: 4px;
            background: rgba(255,255,255,0.2);
            border-radius: 2px;
        }
        
        .virtue-meter.positive { background: #10b981; }
        .virtue-meter.negative { background: #ef4444; }
        
        .coaching-panel {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 300px;
            background: rgba(16, 185, 129, 0.95);
            border-radius: 15px;
            padding: 20px;
            transform: translateX(400px);
            transition: transform 0.3s ease;
            z-index: 1000;
            border: 2px solid #10b981;
        }
        
        .coaching-panel.show {
            transform: translateX(0);
        }
        
        .coach-header {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px;
        }
        
        .coach-avatar {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: #1a202c;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #10b981;
            font-weight: bold;
        }
        
        .controls {
            position: fixed;
            bottom: 20px;
            left: 20px;
            display: flex;
            gap: 15px;
            z-index: 1000;
        }
        
        .control-btn {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            border: none;
            background: rgba(59, 130, 246, 0.9);
            color: white;
            font-size: 1.2em;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .control-btn:hover {
            transform: scale(1.1);
            background: #3b82f6;
        }
        
        .control-btn.recording {
            background: #ef4444;
            animation: pulse 1s infinite;
        }
        
        .progress-tracker {
            position: fixed;
            top: 80px;
            right: 20px;
            width: 250px;
            background: rgba(0,0,0,0.8);
            border-radius: 15px;
            padding: 20px;
            border: 1px solid rgba(255,255,255,0.1);
            z-index: 999;
        }
        
        .virtue-progress {
            margin-bottom: 15px;
        }
        
        .virtue-name {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
            font-size: 0.9em;
        }
        
        .virtue-bar {
            width: 100%;
            height: 6px;
            background: rgba(255,255,255,0.1);
            border-radius: 3px;
            overflow: hidden;
        }
        
        .virtue-fill {
            height: 100%;
            border-radius: 3px;
            transition: width 0.5s ease;
        }
        
        .wisdom-fill { background: #d4af37; }
        .justice-fill { background: #10b981; }
        .courage-fill { background: #ef4444; }
        .temperance-fill { background: #3b82f6; }
        
        .btn {
            padding: 12px 25px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 1em;
            font-weight: 600;
            transition: all 0.3s;
            margin: 5px;
        }
        
        .btn-start {
            background: linear-gradient(45deg, #d4af37, #ffd700);
            color: #1a202c;
            font-size: 1.2em;
            padding: 15px 30px;
        }
        
        .btn-start:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(212,175,55,0.3);
        }
        
        .notification-system {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 1001;
        }
        
        .notification {
            background: rgba(16, 185, 129, 0.95);
            color: white;
            padding: 15px 25px;
            border-radius: 10px;
            margin-bottom: 10px;
            transform: translateY(-100px);
            transition: transform 0.3s ease;
            border: 1px solid #10b981;
            max-width: 400px;
            text-align: center;
        }
        
        .notification.show {
            transform: translateY(0);
        }
        
        .typing-indicator {
            display: flex;
            align-items: center;
            gap: 5px;
            opacity: 0.7;
            font-style: italic;
        }
        
        .typing-dots {
            display: flex;
            gap: 3px;
        }
        
        .typing-dot {
            width: 4px;
            height: 4px;
            border-radius: 50%;
            background: #ffd700;
            animation: typingBounce 1.4s infinite;
        }
        
        .typing-dot:nth-child(2) { animation-delay: 0.2s; }
        .typing-dot:nth-child(3) { animation-delay: 0.4s; }
        
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
        
        @keyframes blink {
            0%, 50% { opacity: 1; }
            51%, 100% { opacity: 0.3; }
        }
        
        @keyframes slideInUp {
            from { opacity: 0; transform: translateY(50px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes messageAppear {
            from { opacity: 0; transform: translateX(-20px); }
            to { opacity: 1; transform: translateX(0); }
        }
        
        @keyframes avatarPulse {
            0%, 100% { border: 2px solid transparent; }
            50% { border: 2px solid #ffd700; }
        }
        
        @keyframes typingBounce {
            0%, 60%, 100% { transform: translateY(0); }
            30% { transform: translateY(-10px); }
        }
        
        /* Mobile responsiveness */
        @media (max-width: 768px) {
            .container {
                padding: 0 15px;
            }
            
            .scenario-grid {
                grid-template-columns: 1fr;
            }
            
            .header {
                padding: 10px 15px;
                flex-direction: column;
                gap: 10px;
            }
            
            .progress-tracker {
                position: relative;
                top: 0;
                right: 0;
                width: 100%;
                margin-bottom: 20px;
            }
            
            .coaching-panel {
                position: relative;
                bottom: 0;
                right: 0;
                width: 100%;
                margin-top: 20px;
                transform: none;
            }
            
            .controls {
                position: relative;
                bottom: 0;
                left: 0;
                justify-content: center;
                margin-top: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <div class="logo" onclick="showWelcome()">
            <div class="logo-symbol">🏛️</div>
            <div>
                <div style="font-size: 1.3em; font-weight: bold; color: #ffd700;">LifeTalk Academy</div>
                <div style="font-size: 0.9em; opacity: 0.8;">"Interactive Stoic Communication"</div>
            </div>
        </div>
        <div class="status">
            <div class="live-indicator">
                <div class="live-dot"></div>
                <span>Marcus AI Online</span>
            </div>
            <div style="color: #d4af37;">⭐ Virtue Level: Sage-in-Training</div>
        </div>
    </div>

    <div class="container">
        <div class="scenario-selector" id="scenarioSelector">
            <div class="scenario-title">🎭 Choose Your Challenge</div>
            <div style="text-align: center; margin-bottom: 30px; opacity: 0.9;">
                Select a scenario to practice Stoic communication principles in real-time
            </div>
            
            <div class="scenario-grid">
                <div class="scenario-card" onclick="selectScenario('angry-boss')" data-scenario="angry-boss">
                    <div class="scenario-emoji">😠</div>
                    <div class="scenario-name">The Furious Manager</div>
                    <div class="scenario-description">Your boss is having a meltdown about the quarterly report. Practice staying virtuous un
