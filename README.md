<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>思政速通 · 刷题练习</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
            background: #f5f7fb;
            color: #1e293b;
            padding: 16px;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: flex-start;
        }
        .container {
            max-width: 800px;
            width: 100%;
            background: white;
            border-radius: 28px;
            box-shadow: 0 20px 40px -12px rgba(0,0,0,0.15);
            padding: 20px 20px 28px;
        }
        h1 {
            font-size: 24px;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 6px;
            margin-bottom: 4px;
            color: #0f172a;
        }
        h1 small { font-size: 15px; font-weight: 400; color: #64748b; margin-left: 6px; }
        .sub {
            color: #475569;
            font-size: 14px;
            border-bottom: 1px solid #e9edf2;
            padding-bottom: 14px;
            margin-bottom: 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
        }
        .stats {
            background: #f1f5f9;
            padding: 4px 14px;
            border-radius: 40px;
            font-size: 13px;
            font-weight: 500;
            color: #334155;
        }
        .mode-tabs {
            display: flex;
            gap: 6px;
            margin-bottom: 18px;
            flex-wrap: wrap;
            align-items: center;
        }
        .mode-tab {
            padding: 8px 16px;
            border-radius: 40px;
            border: none;
            background: #f1f5f9;
            color: #334155;
            font-weight: 600;
            font-size: 14px;
            cursor: pointer;
            transition: 0.2s;
            flex: 1 0 auto;
            text-align: center;
        }
        .mode-tab:hover { background: #e2e8f0; }
        .mode-tab.active {
            background: #1e293b;
            color: white;
            box-shadow: 0 4px 8px rgba(0,0,0,0.08);
        }
        .mode-tab .badge {
            background: rgba(255,255,255,0.25);
            padding: 0 8px;
            border-radius: 20px;
            font-size: 11px;
            margin-left: 4px;
        }
        .mode-tab.active .badge { background: rgba(255,255,255,0.2); }
        .reset-all-btn {
            background: #ef4444;
            color: white;
            border: none;
            border-radius: 40px;
            padding: 6px 18px;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
            margin-left: auto;
        }
        .reset-all-btn:hover { background: #dc2626; }
        .reset-wrong-btn {
            background: #f59e0b;
            color: white;
            border: none;
            border-radius: 40px;
            padding: 6px 18px;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
            margin-left: 8px;
            display: none;
        }
        .reset-wrong-btn:hover { background: #d97706; }
        .toast {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: #1e293b;
            color: white;
            padding: 12px 28px;
            border-radius: 40px;
            font-weight: 500;
            box-shadow: 0 8px 24px rgba(0,0,0,0.2);
            opacity: 0;
            transition: opacity 0.4s ease;
            pointer-events: none;
            z-index: 999;
        }
        .toast.show { opacity: 1; }
        .question-area { margin-top: 4px; }
        .progress {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 14px;
            color: #64748b;
            margin-bottom: 10px;
        }
        .progress-bar {
            height: 4px;
            background: #e2e8f0;
            border-radius: 4px;
            flex: 1;
            margin: 0 14px;
        }
        .progress-fill {
            height: 100%;
            background: #3b82f6;
            border-radius: 4px;
            width: 0%;
            transition: width 0.25s;
        }
        .q-text {
            font-size: 18px;
            font-weight: 600;
            line-height: 1.6;
            margin-bottom: 16px;
            padding: 4px 0;
        }
        .q-type { font-size: 14px; font-weight: 400; color: #64748b; margin-left: 10px; }
        .options {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 20px;
        }
        .option {
            background: #f8fafc;
            border: 2px solid #e9edf2;
            border-radius: 16px;
            padding: 14px 16px;
            cursor: pointer;
            transition: 0.15s;
            font-size: 15px;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .option:hover:not(.disabled) { border-color: #94a3b8; background: #f1f5f9; }
        .option .letter {
            background: #e9edf2;
            border-radius: 40px;
            width: 28px;
            height: 28px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            font-size: 14px;
            color: #1e293b;
            flex-shrink: 0;
        }
        .option.selected { border-color: #3b82f6; background: #eff6ff; }
        .option.selected .letter { background: #3b82f6; color: white; }
        .option.correct { border-color: #22c55e; background: #f0fdf4; }
        .option.correct .letter { background: #22c55e; color: white; }
        .option.wrong { border-color: #ef4444; background: #fef2f2; }
        .option.wrong .letter { background: #ef4444; color: white; }
        .option.disabled { cursor: default; opacity: 0.8; }
        .option .label { flex: 1; }
        .action-bar {
            display: flex;
            gap: 8px;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            margin-top: 6px;
        }
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 40px;
            font-weight: 600;
            font-size: 15px;
            background: #e9edf2;
            color: #1e293b;
            cursor: pointer;
            transition: 0.15s;
            flex: 0 1 auto;
        }
        .btn-primary { background: #1e293b; color: white; }
        .btn-primary:hover:not(:disabled) { background: #0f172a; }
        .btn-primary:disabled { opacity: 0.4; cursor: not-allowed; }
        .btn-outline { background: transparent; border: 2px solid #cbd5e1; }
        .btn-outline:hover { background: #f1f5f9; }
        .result-hint {
            padding: 14px 18px;
            border-radius: 16px;
            margin-bottom: 16px;
            font-weight: 500;
            display: none;
        }
        .result-hint.show { display: block; }
        .result-hint.correct { background: #dcfce7; color: #166534; border: 1px solid #bbf7d0; }
        .result-hint.wrong { background: #fee2e2; color: #991b1b; border: 1px solid #fecaca; }
        .result-hint .answer-detail { font-weight: 400; margin-top: 4px; font-size: 14px; }
        .empty-state { text-align: center; padding: 48px 20px; color: #94a3b8; }
        .empty-state .icon { font-size: 48px; margin-bottom: 12px; }
        .empty-state p { font-size: 16px; }
        .footer-note {
            margin-top: 20px;
            font-size: 13px;
            color: #94a3b8;
            text-align: center;
            border-top: 1px solid #e9edf2;
            padding-top: 16px;
        }
        .doc-content {
            background: #f8fafc;
            border-radius: 16px;
            padding: 18px 20px;
            font-size: 14px;
            line-height: 1.8;
            white-space: pre-wrap;
            word-break: break-word;
            max-height: 600px;
            overflow-y: auto;
            border: 1px solid #e9edf2;
            color: #1e293b;
        }
        @media (max-width: 600px) {
            .container { padding: 14px; }
            .q-text { font-size: 16px; }
            .option { padding: 12px 14px; font-size: 14px; }
            .mode-tab { font-size: 13px; padding: 6px 12px; }
            .btn { padding: 8px 14px; font-size: 14px; }
            .reset-all-btn, .reset-wrong-btn { font-size: 12px; padding: 4px 12px; }
        }
    </style>
</head>
<body>
<div class="container" id="app">
    <div id="toast" class="toast"></div>
    <h1>📚 思政速通 <small>· 刷题练习</small></h1>
    <div class="sub">
        <span>基于复习文档 · 单选/多选</span>
        <span class="stats">📊 错题 <span id="wrongCount">0</span></span>
    </div>

    <div class="mode-tabs">
        <button class="mode-tab active" data-mode="core">🎯 精炼练习 <span class="badge" id="coreCount">0</span></button>
        <button class="mode-tab" data-mode="all">📖 全部练习 <span class="badge" id="allCount">0</span></button>
        <button class="mode-tab" data-mode="wrong">❌ 错题集 <span class="badge" id="wrongCountBadge">0</span></button>
        <button class="mode-tab" data-mode="doc">📄 知识点</button>
        <button class="reset-all-btn" id="resetAllBtn">🔄 重置进度</button>
        <button class="reset-wrong-btn" id="resetWrongBtn">🔁 重置错题进度</button>
    </div>

    <div class="question-area" id="contentArea"></div>
    <div class="footer-note">✅ 错题集：答对后点击下一题移除 · 移除后保持原顺序</div>
</div>

<script>
// ================================================================
// 1. 题库（40题）
// ================================================================
const allQuestions = [
    { id:1, core:true, type:'single', question:'人的本质是什么？', options:['A. 理性与自由意志的统一','B. 一切社会关系的总和','C. 生物性与社会性的混合','D. 先天遗传与后天教育的产物'], answer:1 },
    { id:2, core:true, type:'single', question:'人生观的核心是什么？', options:['A. 人生目的','B. 人生态度','C. 人生价值','D. 人生理想'], answer:0 },
    { id:3, core:true, type:'single', question:'评价人生价值的根本尺度是看一个人的实践活动是否（　）', options:['A. 符合个人利益','B. 符合社会发展的客观规律，促进历史进步','C. 获得社会多数人的认可','D. 实现自我价值最大化'], answer:1 },
    { id:4, core:true, type:'single', question:'下列哪项不属于评价人生价值应坚持的原则？', options:['A. 能力有大小与贡献须尽力相统一','B. 物质贡献与精神贡献相统一','C. 个人利益与集体利益相统一','D. 动机与效果相统一'], answer:2 },
    { id:5, core:true, type:'single', question:'理想信念的作用不包括以下哪项？', options:['A. 昭示奋斗目标','B. 催生前进动力','C. 提供物质保障','D. 提高精神境界'], answer:2 },
    { id:6, core:true, type:'single', question:'个人与社会的关系中，最根本的是（　）的关系。', options:['A. 个人权利与社会义务','B. 个人自由与社会秩序','C. 个人利益与社会利益','D. 个人发展与社会发展'], answer:2 },
    { id:7, core:true, type:'single', question:'中国精神是民族精神与（　）的统一。', options:['A. 时代精神','B. 创新精神','C. 奋斗精神','D. 团结精神'], answer:0 },
    { id:8, core:true, type:'single', question:'时代精神的核心是（　）', options:['A. 爱国主义','B. 集体主义','C. 改革创新','D. 艰苦奋斗'], answer:2 },
    { id:9, core:true, type:'single', question:'当代中国爱国主义的本质是坚持爱党、爱国和（　）高度统一。', options:['A. 爱社会主义','B. 爱人民','C. 爱文化','D. 爱和平'], answer:0 },
    { id:10, core:true, type:'single', question:'社会主义核心价值观在国家层面的价值目标是（　）', options:['A. 富强、民主、文明、和谐','B. 自由、平等、公正、法治','C. 爱国、敬业、诚信、友善','D. 富强、平等、公正、法治'], answer:0 },
    { id:11, core:true, type:'single', question:'道德起源的首要前提是（　）', options:['A. 社会关系','B. 劳动','C. 人的自我意识','D. 语言'], answer:1 },
    { id:12, core:true, type:'single', question:'社会主义道德的核心是（　）', options:['A. 集体主义','B. 为人民服务','C. 爱国主义','D. 诚实守信'], answer:1 },
    { id:13, core:true, type:'single', question:'社会主义道德的基本原则是（　）', options:['A. 集体主义','B. 个人主义','C. 功利主义','D. 人道主义'], answer:0 },
    { id:14, core:true, type:'single', question:'集体主义原则强调国家利益、社会整体利益和（　）的辩证统一。', options:['A. 个人利益','B. 集体利益','C. 企业利益','D. 家庭利益'], answer:0 },
    { id:15, core:true, type:'single', question:'新时代公民道德建设的着力点不包括（　）', options:['A. 社会公德','B. 职业道德','C. 网络道德','D. 家庭美德'], answer:2 },
    { id:16, core:true, type:'single', question:'社会公德的主要内容不包括（　）', options:['A. 文明礼貌','B. 助人为乐','C. 爱岗敬业','D. 遵纪守法'], answer:2 },
    { id:17, core:false, type:'single', question:'职业道德的基本要求不包括（　）', options:['A. 爱岗敬业','B. 诚实守信','C. 保护环境','D. 奉献社会'], answer:2 },
    { id:18, core:false, type:'single', question:'法律运行主要包括立法、执法、司法和（　）', options:['A. 守法','B. 法律监督','C. 法律解释','D. 法律宣传'], answer:0 },
    { id:19, core:false, type:'single', question:'中国特色社会主义法律体系以（　）为统帅。', options:['A. 刑法','B. 民法','C. 宪法','D. 行政法'], answer:2 },
    { id:20, core:false, type:'single', question:'我国第一部宪法颁布于（　）年。', options:['A. 1949','B. 1954','C. 1978','D. 1982'], answer:1 },
    { id:21, core:false, type:'single', question:'全面依法治国的总目标是建设中国特色社会主义法治体系，建设（　）', options:['A. 社会主义法治国家','B. 法治政府','C. 法治社会','D. 法治中国'], answer:0 },
    { id:22, core:false, type:'single', question:'法律体系层次从高到低依次为宪法、法律、（　）和地方性法规。', options:['A. 行政法规','B. 部门规章','C. 国际条约','D. 自治条例'], answer:0 },
    { id:23, core:false, type:'single', question:'我国社会主义法律的本质特征包括体现了党的主张和（　）的统一。', options:['A. 人民意志','B. 政府意志','C. 社会意志','D. 个人意志'], answer:0 },
    { id:24, core:false, type:'single', question:'民事主体包括自然人、法人和（　）', options:['A. 非法人组织','B. 合伙企业','C. 个体工商户','D. 农村承包经营户'], answer:0 },
    { id:25, core:false, type:'single', question:'法治思维的核心内容不包括（　）', options:['A. 法律至上','B. 权力制约','C. 效率优先','D. 公平正义'], answer:2 },
    { id:26, core:false, type:'single', question:'信仰是最高层次的（　），具有最大统摄力。', options:['A. 信念','B. 理想','C. 价值观','D. 世界观'], answer:0 },
    { id:27, core:false, type:'single', question:'下列哪项不属于中国精神的丰富内涵？', options:['A. 伟大创造精神','B. 伟大团结精神','C. 伟大斗争精神','D. 伟大梦想精神'], answer:2 },
    { id:28, core:false, type:'single', question:'爱国主义的基本要求包括爱祖国的大好河山、爱自己的骨肉同胞、爱祖国的灿烂文化和（　）', options:['A. 爱自己的国家','B. 爱中国共产党','C. 爱社会主义','D. 爱人民'], answer:0 },
    { id:29, core:false, type:'single', question:'社会公德是维护公共利益、公共秩序和社会和谐稳定的（　）道德要求。', options:['A. 最高','B. 起码','C. 核心','D. 一般'], answer:1 },
    { id:30, core:false, type:'single', question:'职业道德的特点包括鲜明的行业性、适用范围的有限性、表现形式的多样性和（　）', options:['A. 一定的强制性','B. 高度的自律性','C. 广泛的社会性','D. 历史的继承性'], answer:0 },
    { id:31, core:false, type:'single', question:'公共生活具有开放性与透明性，对社会的影响（　）', options:['A. 更直接广泛','B. 更间接有限','C. 基本没有','D. 取决于个人'], answer:0 },
    { id:32, core:false, type:'single', question:'道德的社会职能包括认识、规范和（　）', options:['A. 调节','B. 教育','C. 引导','D. 惩罚'], answer:0 },
    { id:33, core:false, type:'single', question:'社会主义道德建设要以为人民服务为核心，以（　）为原则。', options:['A. 集体主义','B. 个人主义','C. 人道主义','D. 利他主义'], answer:0 },
    { id:34, core:false, type:'single', question:'法律由国家制定或认可，并以（　）保证实施。', options:['A. 国家强制力','B. 社会舆论','C. 道德约束','D. 行政命令'], answer:0 },
    { id:35, core:false, type:'single', question:'宪法是国家的根本法，具有（　）法律效力。', options:['A. 最高','B. 一般','C. 特殊','D. 较低'], answer:0 },
    { id:36, core:false, type:'single', question:'下列哪项属于法治思维中“权利保障”的内涵？', options:['A. 保障公民合法权益','B. 限制政府权力','C. 维护社会稳定','D. 提高司法效率'], answer:0 },
    // 多选
    { id:37, core:true, type:'multi', question:'评价人生价值应坚持的原则包括（　）', options:['A. 能力有大小与贡献须尽力相统一','B. 物质贡献与精神贡献相统一','C. 完善自身与贡献社会相统一','D. 动机与效果相统一'], answer:[0,1,2,3] },
    { id:38, core:true, type:'multi', question:'理想信念的作用体现在（　）', options:['A. 昭示奋斗目标','B. 催生前进动力','C. 提供精神支柱','D. 提高精神境界'], answer:[0,1,2,3] },
    { id:39, core:false, type:'multi', question:'社会公德的主要内容有（　）', options:['A. 文明礼貌','B. 助人为乐','C. 爱护公物','D. 保护环境','E. 遵纪守法'], answer:[0,1,2,3,4] },
    { id:40, core:false, type:'multi', question:'职业道德的基本要求包括（　）', options:['A. 爱岗敬业','B. 诚实守信','C. 办事公道','D. 热情服务','E. 奉献社会'], answer:[0,1,2,3,4] }
];

// ================================================================
// 2. 知识点
// ================================================================
const knowledgeDoc = `
【人的本质与人生观】
人的本质：不是单个人所固有的抽象物，在其现实性上，它是一切社会关系的总和。
人生观主要内容：人生目的（核心）、人生态度、人生价值。
评价人生价值的根本尺度：看实践活动是否符合社会发展的客观规律，是否促进了历史的进步。
评价人生价值应坚持的原则：能力有大小与贡献须尽力相统一、物质贡献与精神贡献相统一、完善自身与贡献社会相统一、动机与效果相统一。

【理想与信念】
理想类型：个人理想与社会理想；长期理想与近期理想；政治理想、道德理想、生活理想、职业理想。
信仰是最高层次的信念，具有最大统摄力。
个人与社会的关系最根本的是个人利益与社会利益的关系。
理想信念作用：昭示奋斗目标，催生前进动力，提供精神支柱，提高精神境界。

【中国精神与爱国主义】
中国精神是民族精神与时代精神的统一，时代精神的核心是改革创新。
中国精神丰富内涵：伟大创造、团结、梦想、奋斗精神。
爱国主义基本要求：爱祖国的大好河山、爱自己的骨肉同胞、爱祖国的灿烂文化、爱自己的国家。
当代中国爱国主义的本质：坚持爱党与爱国、爱社会主义高度统一。

【社会主义核心价值观】
三个层面：国家（富强、民主、文明、和谐）、社会（自由、平等、公正、法治）、个人（爱国、敬业、诚信、友善）。

【道德观】
劳动是道德起源的首要前提，社会关系是客观条件，人的自我意识是主观条件。
道德的职能：认识、规范、调节。

【社会主义道德】
核心：为人民服务。
原则：集体主义（调节国家利益、社会整体利益与个人利益关系的基本原则）。
集体主义内涵：强调三者利益的辩证统一；强调国家、社会整体利益高于个人利益。

【公民道德建设】
着力点：社会公德、职业道德、家庭美德、个人品德。

【社会公德】
定义：维护公共利益、公共秩序、社会和谐稳定的起码道德要求，涵盖人与人、人与社会、人与自然。
主要内容：文明礼貌、助人为乐、爱护公物、保护环境、遵纪守法。

【职业道德】
基本要求：爱岗敬业、诚实守信、办事公道、热情服务、奉献社会。
特点：鲜明的行业性、适用范围的有限性、表现形式多样性、一定的强制性。

【法律基础】
法律：由国家制定或认可，以国家强制力保证实施，反映统治阶级意志的规范体系。
法律运行：立法、执法、司法、守法。
中国特色社会主义法律体系以宪法为统帅，宪法是根本法，最高效力。
第一部宪法：1954年9月20日。
全面依法治国总目标：建设中国特色社会主义法治体系，建设社会主义法治国家。
法律体系层次：宪法 → 法律 → 行政法规 → 地方性法规。
社会主义法律本质特征：体现党的主张和人民意志的统一；科学性与先进性的统一；重要保障。
民事主体：自然人、法人、非法人组织。
法治思维核心：法律至上、权力制约、公平正义、权利保障。
`;

// ================================================================
// 3. 状态
// ================================================================
let currentMode = 'core';
let currentIndex = 0;
let currentQuestions = [];

let userAnswers = JSON.parse(localStorage.getItem('userAnswers') || '{}');
let answered = JSON.parse(localStorage.getItem('answered') || '{}');
let wrongIds = new Set(JSON.parse(localStorage.getItem('wrongIds') || '[]'));
let pendingRemoveIds = new Set();

function saveState() {
    localStorage.setItem('userAnswers', JSON.stringify(userAnswers));
    localStorage.setItem('answered', JSON.stringify(answered));
    localStorage.setItem('wrongIds', JSON.stringify([...wrongIds]));
}

function updateStats() {
    document.getElementById('wrongCount').textContent = wrongIds.size;
    document.getElementById('wrongCountBadge').textContent = wrongIds.size;
    const coreList = allQuestions.filter(q => q.core);
    document.getElementById('coreCount').textContent = coreList.length;
    document.getElementById('allCount').textContent = allQuestions.length;
}

function shuffle(arr) {
    for (let i = arr.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]];
    }
    return arr;
}

function generateQuestions(mode) {
    let pool = [];
    if (mode === 'core') pool = allQuestions.filter(q => q.core);
    else if (mode === 'all') pool = [...allQuestions];
    else if (mode === 'wrong') pool = allQuestions.filter(q => wrongIds.has(q.id));
    else pool = [];
    currentQuestions = shuffle(pool);
    currentIndex = 0;
    pendingRemoveIds.clear();
}

// 从错题集移除指定 id 的题目（同时从列表和 wrongIds 中删除），保持顺序
function removeWrongQuestions(ids) {
    if (ids.size === 0) return;
    for (let id of ids) wrongIds.delete(id);
    const oldIdx = currentIndex;
    const oldQuestions = currentQuestions;
    const currentId = (oldQuestions.length > 0 && oldIdx < oldQuestions.length) ? oldQuestions[oldIdx].id : null;
    // 过滤
    currentQuestions = currentQuestions.filter(q => !ids.has(q.id));
    if (currentQuestions.length === 0) {
        currentIndex = 0;
        pendingRemoveIds.clear();
        saveState();
        return;
    }
    // 判断当前题是否被移除
    if (currentId !== null && ids.has(currentId)) {
        // 当前题被移除：后面的前移，新索引应为原索引（不超出）
        if (oldIdx < currentQuestions.length) {
            currentIndex = oldIdx;
        } else {
            currentIndex = currentQuestions.length - 1;
        }
    } else {
        // 当前题未被移除：查找其在新列表中的位置
        if (currentId !== null) {
            const newIdx = currentQuestions.findIndex(q => q.id === currentId);
            if (newIdx !== -1) {
                currentIndex = newIdx;
            } else {
                // 安全兜底
                currentIndex = Math.min(oldIdx, currentQuestions.length - 1);
            }
        } else {
            currentIndex = Math.min(oldIdx, currentQuestions.length - 1);
        }
    }
    if (currentIndex < 0) currentIndex = 0;
    pendingRemoveIds.clear();
    saveState();
}

// 显示非阻塞提示
function showToast(text, duration = 2500) {
    const el = document.getElementById('toast');
    el.textContent = text;
    el.classList.add('show');
    clearTimeout(el._timer);
    el._timer = setTimeout(() => el.classList.remove('show'), duration);
}

// 重置进度（清空所有答题记录，并重新随机打乱当前模式题目）
function resetProgress() {
    if (!confirm('确定要重置所有题目的答题记录吗？（错题集将保留）')) return;
    userAnswers = {};
    answered = {};
    saveState();
    generateQuestions(currentMode);
    renderContent();
    showToast('已重置进度，题目顺序已打乱');
}

// 重置错题进度（清空错题集的答题记录，并重新打乱错题顺序）
function resetWrongProgress() {
    if (currentMode !== 'wrong') return;
    if (!confirm('确定要重置错题集的所有答题记录吗？（错题列表保持不变，顺序将重新打乱）')) return;
    const wrongQids = new Set(allQuestions.filter(q => wrongIds.has(q.id)).map(q => q.id));
    for (let qid of wrongQids) {
        delete userAnswers[qid];
        delete answered[qid];
    }
    saveState();
    generateQuestions('wrong');
    renderContent();
    showToast('错题进度已重置，顺序已打乱');
}

// ================================================================
// 渲染
// ================================================================
function renderContent() {
    const area = document.getElementById('contentArea');

    if (currentMode === 'doc') {
        area.innerHTML = `
            <div style="margin-bottom:12px; font-weight:600; font-size:18px;">📖 思政复习知识点</div>
            <div class="doc-content">${knowledgeDoc}</div>
        `;
        document.getElementById('resetWrongBtn').style.display = 'none';
        return;
    }

    document.getElementById('resetWrongBtn').style.display = (currentMode === 'wrong') ? 'inline-block' : 'none';

    if (currentQuestions.length === 0 || (currentMode === 'wrong' && currentQuestions.some(q => !wrongIds.has(q.id)))) {
        generateQuestions(currentMode);
    }

    const questions = currentQuestions;
    if (questions.length === 0) {
        area.innerHTML = `
            <div class="empty-state">
                <div class="icon">📭</div>
                <p>当前模式暂无题目</p>
                <p style="font-size:14px; margin-top:8px;">切换其他模式试试</p>
            </div>
        `;
        return;
    }

    if (currentIndex >= questions.length) currentIndex = questions.length - 1;
    if (currentIndex < 0) currentIndex = 0;
    const q = questions[currentIndex];
    const total = questions.length;
    const progress = ((currentIndex + 1) / total * 100).toFixed(0);

    const hasAnswered = answered[q.id] || false;
    const selectedIndices = userAnswers[q.id] || [];

    const letters = 'ABCDEFGHIJ'.split('');
    let optionsHtml = q.options.map((opt, idx) => {
        let classes = 'option';
        if (hasAnswered) {
            classes += ' disabled';
            const isCorrect = q.type === 'single' ? (idx === q.answer) : (q.answer.includes(idx));
            if (isCorrect) classes += ' correct';
            if (selectedIndices.includes(idx) && !isCorrect) classes += ' wrong';
            if (selectedIndices.includes(idx)) classes += ' selected';
        } else {
            if (selectedIndices.includes(idx)) classes += ' selected';
        }
        return `<div class="${classes}" data-index="${idx}" data-qid="${q.id}">
            <span class="letter">${letters[idx]}</span>
            <span class="label">${opt.substring(3)}</span>
        </div>`;
    }).join('');

    let hintHtml = '';
    if (hasAnswered) {
        let isCorrect;
        if (q.type === 'single') {
            isCorrect = (selectedIndices.length === 1 && selectedIndices[0] === q.answer);
        } else {
            const sortedAns = [...q.answer].sort();
            const sortedSel = [...selectedIndices].sort();
            isCorrect = (sortedAns.length === sortedSel.length && sortedAns.every((v,i) => v === sortedSel[i]));
        }
        let correctAnswerStr = q.type === 'single' ? letters[q.answer] : q.answer.map(i => letters[i]).join('、');
        hintHtml = `
            <div class="result-hint show ${isCorrect ? 'correct' : 'wrong'}">
                ${isCorrect ? '✅ 回答正确！' : '❌ 回答错误。正确答案是 ' + correctAnswerStr}
                ${q.type === 'multi' ? '<div class="answer-detail">（多选题，需全部选对）</div>' : ''}
                ${(currentMode === 'wrong' && isCorrect && wrongIds.has(q.id)) ? '<div class="answer-detail">⏳ 点击“下一题”后将从错题集移除</div>' : ''}
            </div>
        `;
    }

    const progressHtml = `
        <div class="progress">
            <span>第 ${currentIndex+1} / ${total} 题</span>
            <div class="progress-bar"><div class="progress-fill" style="width:${progress}%;"></div></div>
            <span>${progress}%</span>
        </div>
    `;

    const typeLabel = q.type === 'single' ? '【单选】' : '【多选】';

    let actionHtml = '';
    if (!hasAnswered) {
        actionHtml = `
            <div class="action-bar">
                <button class="btn btn-outline" id="prevBtn" ${currentIndex===0?'disabled':''}>← 上一题</button>
                <button class="btn btn-primary" id="submitBtn" ${selectedIndices.length===0?'disabled':''}>确认答案</button>
                <button class="btn btn-outline" id="resetBtn">重置当前</button>
            </div>
        `;
    } else {
        actionHtml = `
            <div class="action-bar">
                <button class="btn btn-outline" id="prevBtn" ${currentIndex===0?'disabled':''}>← 上一题</button>
                <button class="btn btn-primary" id="nextBtn">${currentIndex===total-1?'完成':'下一题 →'}</button>
                <button class="btn btn-outline" id="resetBtn">重置当前</button>
            </div>
        `;
    }

    area.innerHTML = progressHtml + `
        <div class="q-text">${q.question} <span class="q-type">${typeLabel}</span></div>
        <div class="options" id="optionsContainer">${optionsHtml}</div>
        ${hintHtml}
        ${actionHtml}
    `;

    // ---- 事件绑定 ----
    // 上一题
    const prevBtn = document.getElementById('prevBtn');
    if (prevBtn) {
        prevBtn.addEventListener('click', function() {
            if (currentIndex > 0) { currentIndex--; renderContent(); }
        });
    }

    // 选项点击
    if (!hasAnswered) {
        document.querySelectorAll('.option').forEach(el => {
            el.addEventListener('click', function() {
                if (this.classList.contains('disabled')) return;
                const qid = parseInt(this.dataset.qid);
                const idx = parseInt(this.dataset.index);
                const curQ = currentQuestions.find(q => q.id === qid);
                if (!curQ) return;
                let currentSelected = userAnswers[qid] || [];
                if (curQ.type === 'single') {
                    currentSelected = [idx];
                } else {
                    if (currentSelected.includes(idx)) {
                        currentSelected = currentSelected.filter(i => i !== idx);
                    } else {
                        currentSelected.push(idx);
                    }
                }
                userAnswers[qid] = currentSelected;
                saveState();
                renderContent();
            });
        });
    }

    // 提交
    const submitBtn = document.getElementById('submitBtn');
    if (submitBtn) {
        submitBtn.addEventListener('click', function() {
            const qid = q.id;
            const selected = userAnswers[qid] || [];
            if (selected.length === 0) return;
            answered[qid] = true;
            let isCorrect;
            if (q.type === 'single') {
                isCorrect = (selected.length === 1 && selected[0] === q.answer);
            } else {
                const sortedAns = [...q.answer].sort();
                const sortedSel = [...selected].sort();
                isCorrect = (sortedAns.length === sortedSel.length && sortedAns.every((v,i) => v === sortedSel[i]));
            }
            const isWrong = wrongIds.has(qid);
            if (isCorrect && isWrong) {
                if (currentMode === 'wrong') {
                    pendingRemoveIds.add(qid);
                } else {
                    wrongIds.delete(qid);
                }
            } else if (!isCorrect && !isWrong) {
                wrongIds.add(qid);
            }
            saveState();
            updateStats();
            renderContent();
        });
    }

    // 重置当前
    const resetBtn = document.getElementById('resetBtn');
    if (resetBtn) {
        resetBtn.addEventListener('click', function() {
            delete userAnswers[q.id];
            delete answered[q.id];
            if (pendingRemoveIds.has(q.id)) pendingRemoveIds.delete(q.id);
            saveState();
            renderContent();
        });
    }

    // 下一题/完成
    const nextBtn = document.getElementById('nextBtn');
    if (nextBtn) {
        nextBtn.addEventListener('click', function() {
            // 先移除待移除的错题（保持顺序）
            if (currentMode === 'wrong' && pendingRemoveIds.size > 0) {
                removeWrongQuestions(pendingRemoveIds);
                if (currentQuestions.length === 0) {
                    renderContent();
                    return;
                }
                // 若当前索引超出，修正
                if (currentIndex >= currentQuestions.length) {
                    currentIndex = currentQuestions.length - 1;
                }
            }
            // 跳转
            if (currentIndex < currentQuestions.length - 1) {
                currentIndex++;
                renderContent();
            } else {
                // 完成所有题目：非阻塞提示，然后回到第一题
                showToast('🎉 已完成所有题目！');
                currentIndex = 0;
                renderContent();
            }
        });
    }

    updateStats();
}

// ================================================================
// 模式切换
// ================================================================
function setMode(mode) {
    currentMode = mode;
    document.querySelectorAll('.mode-tab').forEach(tab => {
        tab.classList.toggle('active', tab.dataset.mode === mode);
    });
    pendingRemoveIds.clear();
    generateQuestions(mode);
    renderContent();
}

// ================================================================
// 事件绑定
// ================================================================
document.querySelectorAll('.mode-tab').forEach(tab => {
    tab.addEventListener('click', function() { setMode(this.dataset.mode); });
});
document.getElementById('resetAllBtn').addEventListener('click', resetProgress);
document.getElementById('resetWrongBtn').addEventListener('click', resetWrongProgress);

// ================================================================
// 初始化
// ================================================================
updateStats();
document.querySelector('.mode-tab[data-mode="core"]').classList.add('active');
setMode('core');
</script>
</body>
</html>
