<script src="https://gist.github.com/wesleyferreira-oss/1a38b84c7afe4b534396af134797523a.js"></script><!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador de RV - Novo Modelo (Ago/25)</title>
    <style>
        /* --- GERAL E LAYOUT --- */
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap');

        body {
            font-family: 'Roboto', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #eef1f5;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 30px;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        .calculator-container {
            background-color: #ffffff;
            padding: 50px;
            border-radius: 20px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.08);
            width: 100%;
            max-width: 700px;
            transition: all 0.3s ease;
        }

        .logo {
            display: block;
            margin: 0 auto 30px auto;
            max-width: 350px;
            width: 100%;
            height: auto;
        }

        h1 {
            text-align: center;
            color: #005a9c;
            font-size: 2em;
            font-weight: 700;
            margin: 0 0 30px 0;
        }

        /* --- FORMULÁRIOS E INPUTS --- */
        .form-section {
            background-color: #f8f9fa;
            border: 1px solid #e9ecef;
            border-radius: 16px;
            padding: 30px;
            margin-top: 30px;
        }

        .form-section h3 {
            color: #005a9c;
            margin-top: 0;
            margin-bottom: 20px;
            font-size: 1.2em;
            border-bottom: 1px solid #e9ecef;
            padding-bottom: 10px;
        }

        .form-group {
            margin-bottom: 25px;
        }
        .form-section .form-group:last-child {
            margin-bottom: 0;
        }
        .calculator-container > .form-group:last-of-type {
             margin-bottom: 0;
        }


        label {
            display: block;
            margin-bottom: 10px;
            font-weight: 500;
            color: #495057;
        }

        input[type="number"], input[type="text"] {
            width: 100%;
            padding: 14px;
            border: 1px solid #ced4da;
            border-radius: 10px;
            box-sizing: border-box;
            font-size: 16px;
            transition: all 0.2s ease;
        }
        input:focus {
            outline: none;
            border-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.15);
        }

        /* --- BOTÕES --- */
        .button-group {
            margin-top: 30px;
        }
        button {
            width: 100%;
            padding: 16px;
            background-image: linear-gradient(45deg, #007bff 0%, #0056b3 100%);
            color: white;
            border: none;
            border-radius: 12px;
            font-size: 18px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0, 123, 255, 0.2);
        }
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 123, 255, 0.3);
        }
        #resetButton {
            background-image: linear-gradient(45deg, #6c757d 0%, #5a6268 100%);
            box-shadow: 0 4px 15px rgba(108, 117, 125, 0.2);
        }
        #resetButton:hover {
            box-shadow: 0 6px 20px rgba(108, 117, 125, 0.3);
        }

        .selection-buttons {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 12px;
        }
        .selection-button {
            padding: 12px 15px;
            border: 1px solid #ced4da;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s ease;
            text-align: center;
            background-color: #fff;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
        }
        .selection-button:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(0,0,0,0.08);
            border-color: #007bff;
        }
        .selection-button.selected {
            background-color: #007bff;
            color: white;
            border-color: #0056b3;
            box-shadow: 0 4px 15px rgba(0, 123, 255, 0.2);
        }

        /* --- METAS CALCULADAS EM DESTAQUE --- */
        .calculated-value-item p {
            margin-bottom: 10px;
            font-weight: 500;
        }
        .calculated-value-item span {
            background-color: #eef1f5;
            color: #005a9c;
            padding: 15px;
            border-radius: 12px;
            border: 1px solid #dee2e6;
            font-size: 1.25em;
            font-weight: 700;
            min-height: 60px;
            box-sizing: border-box;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* --- RESULTADOS --- */
        .results {
            margin-top: 40px;
            border-top: 1px solid #dee2e6;
            padding-top: 20px;
        }

        .report-section {
            border-top: 1px dashed #ced4da;
            margin-top: 30px;
            padding-top: 30px;
        }

        .final-rv {
            font-size: 1.6em;
            color: #28a745;
            font-weight: 700;
        }

        .result-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #eee;
        }
        
        .grid-container {
            margin-top: 30px;
            overflow-x: auto;
            padding-bottom: 15px;
        }
        .grid-container h4 {
            text-align: center;
            margin-bottom: 20px;
        }
        .rv-grid {
            border-collapse: separate;
            border-spacing: 10px;
            width: 100%;
            min-width: 500px;
        }
        .rv-grid th, .rv-grid td {
            border: none;
            padding: 15px;
            text-align: center;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.07);
            transition: all 0.2s ease;
        }
        .rv-grid th {
            background-color: #005a9c;
            color: white;
            font-weight: 500;
        }
        .rv-grid .axis-y {
            background-color: #f8f9fa;
            color: #495057;
            font-weight: 500;
        }
        .rv-grid td {
            background-color: #ffffff;
        }
        .rv-grid .highlight {
            background-color: #d4edda;
            border: 2px solid #28a745;
            transform: scale(1.05);
            box-shadow: 0 6px 20px rgba(40, 167, 69, 0.3);
        }
        
        .info-box {
            background-color: #e9f5ff;
            border-left: 5px solid #007bff;
            border-radius: 8px;
            padding: 20px 25px;
            margin: 30px 0;
            line-height: 1.6;
            font-size: 0.95em;
        }
        
        .hidden-input {
            display: none;
        }
    </style>
</head>
<body>

<div class="calculator-container">
    <img src="https://raw.githubusercontent.com/felipecapelos/MeuHtmlPublico/refs/heads/main/Novo%20Logo%20Azul.png" alt="Logo Azul" class="logo">
    <h1>Simulador de RV</h1>
    
    <div class="form-group">
        <label for="grupo">Escolha seu Grupo:</label>
        <div class="selection-buttons" id="grupo-selection">
            <div class="selection-button" data-value="1">Grupo 1<span class="group-text"><br>(acima de R$250k)</span></div>
            <div class="selection-button" data-value="2">Grupo 2<span class="group-text"><br>(até R$250k)</span></div>
            <div class="selection-button" data-value="3">Grupo 3<span class="group-text"><br>(até R$170k)</span></div>
            <div class="selection-button" data-value="4">Grupo 4<span class="group-text"><br>(até R$130k)</span></div>
        </div>
        <input type="hidden" id="grupo">
    </div>

    <div class="form-group">
        <label for="cargo">Cargo:</label>
        <div class="selection-buttons" id="cargo-selection">
            <div class="selection-button" data-value="Consultora/Especialista">Consultora / Especialista</div>
            <div class="selection-button" data-value="GU">Gerente de Unidade (GU)</div>
        </div>
        <input type="hidden" id="cargo">
    </div>
    
    <div class="form-section">
        <h3>Informações da Loja</h3>
        <div class="form-group">
            <label for="metaLojaMes">Meta da Loja (Mês):</label>
            <input type="text" id="metaLojaMes" value="" placeholder="R$ 0,00">
        </div>
        <div class="form-group">
            <label for="headcount">Headcount Ideal da Loja:</label>
            <div class="selection-buttons" id="headcount-selection">
            </div>
            <input type="hidden" id="headcount">
        </div>
        <div class="form-group">
            <label for="vendaLoja1">Venda Loja 1ª Quinzena:</label>
            <input type="text" id="vendaLoja1" value="" placeholder="R$ 0,00">
        </div>
        <div class="form-group">
            <label for="vendaLoja2">Venda Loja 2ª Quinzena:</label>
            <input type="text" id="vendaLoja2" value="" placeholder="R$ 0,00">
        </div>
    </div>
    
    <div class="form-section">
        <h3>Informações de Vendas e Metas</h3>
        
        <div class="form-group" id="meta-individual-display">
            <label>Meta Individual:</label>
            <div class="calculated-value-container">
                <div class="calculated-value-item">
                    <p>Mês</p>
                    <span id="metaIndividualMes" class="calculated-value">R$ 0,00</span>
                </div>
                <div class="calculated-value-item">
                    <p>Quinzena</p>
                    <span id="metaIndividualQuinzena" class="calculated-value">R$ 0,00</span>
                </div>
            </div>
        </div>

        <div class="form-group hidden-input" id="meta-gu-display">
            <label>Meta da Loja:</label>
            <div class="calculated-value-container">
                <div class="calculated-value-item">
                    <p>Mês</p>
                    <span id="metaLojaMesDisplay" class="calculated-value">R$ 0,00</span>
                </div>
                <div class="calculated-value-item">
                    <p>Quinzena</p>
                    <span id="metaLojaQuinzenaDisplay" class="calculated-value">R$ 0,00</span>
                </div>
            </div>
        </div>

        <div id="venda-inputs-consultor" class="hidden-input">
            <div class="form-group">
                <label for="vendaIndividual1">Venda Individual 1ª Quinzena:</label>
                <input type="text" id="vendaIndividual1" value="" placeholder="R$ 0,00">
            </div>
            <div class="form-group">
                <label for="vendaIndividual2">Venda Individual 2ª Quinzena:</label>
                <input type="text" id="vendaIndividual2" value="" placeholder="R$ 0,00">
            </div>
        </div>
    </div>
    
    <div class="form-section">
        <h3>Indicador Desafio</h3>
        <div class="form-group">
            <label for="npsReal">NPS Real:</label>
            <input type="number" id="npsReal" value="" placeholder="0" min="0" max="100">
        </div>
    </div>
    
    <div class="button-group">
        <button id="calculateButton">Calcular Minha RV</button>
    </div>

    <div class="results" id="results">
    </div>
</div>

<script>
    function setupInteractiveSelection(containerId, hiddenInputId) {
        const container = document.getElementById(containerId);
        if (!container) return;
        
        container.addEventListener('click', (event) => {
            const button = event.target.closest('.selection-button');
            if (!button) return;
            
            const buttons = container.querySelectorAll('.selection-button');
            buttons.forEach(btn => btn.classList.remove('selected'));
            button.classList.add('selected');

            document.getElementById(hiddenInputId).value = button.dataset.value;
            
            const inputEvent = new Event('input', { bubbles: true });
            document.getElementById(hiddenInputId).dispatchEvent(inputEvent);
        });
    }

    window.addEventListener('DOMContentLoaded', () => {
        setupInteractiveSelection('grupo-selection', 'grupo');
        setupInteractiveSelection('cargo-selection', 'cargo');
        setupInteractiveSelection('headcount-selection', 'headcount');

        const headcountContainer = document.getElementById('headcount-selection');
        for (let i = 1; i <= 10; i++) {
            headcountContainer.innerHTML += `<div class="selection-button" data-value="${i}">${i}</div>`;
        }

        document.getElementById('headcount').addEventListener('input', updateCalculatedMetas);
        document.getElementById('metaLojaMes').addEventListener('input', updateCalculatedMetas);
        
        document.getElementById('cargo').addEventListener('input', (event) => {
            const cargo = event.target.value;
            const inputsConsultor = document.getElementById('venda-inputs-consultor');
            const metaIndividualDisplay = document.getElementById('meta-individual-display');
            const metaGUDisplay = document.getElementById('meta-gu-display');
            
            if (cargo === 'Consultora/Especialista') {
                inputsConsultor.style.display = 'block';
                metaIndividualDisplay.style.display = 'block';
                metaGUDisplay.style.display = 'none';
            } else if (cargo === 'GU') {
                inputsConsultor.style.display = 'none';
                metaIndividualDisplay.style.display = 'none';
                metaGUDisplay.style.display = 'block';
            } else {
                inputsConsultor.style.display = 'none';
                metaIndividualDisplay.style.display = 'none';
                metaGUDisplay.style.display = 'none';
            }
            updateCalculatedMetas();
        });
    });
    
    function updateCalculatedMetas() {
        const metaLojaMes = parseCurrency(document.getElementById('metaLojaMes').value);
        const headcountValue = document.getElementById('headcount').value;
        const headcount = parseFloat(headcountValue);
        
        const metaIndividualMesElement = document.getElementById('metaIndividualMes');
        const metaIndividualQuinzenaElement = document.getElementById('metaIndividualQuinzena');
        
        const metaLojaMesDisplayElement = document.getElementById('metaLojaMesDisplay');
        const metaLojaQuinzenaDisplayElement = document.getElementById('metaLojaQuinzenaDisplay');

        if (headcount > 0 && !isNaN(metaLojaMes) && metaLojaMes > 0) {
            const calculatedMetaMes = metaLojaMes / headcount;
            const calculatedMetaQuinzena = calculatedMetaMes / 2;
            metaIndividualMesElement.innerText = formatCurrency(calculatedMetaMes);
            metaIndividualQuinzenaElement.innerText = formatCurrency(calculatedMetaQuinzena);
        } else {
            metaIndividualMesElement.innerText = 'R$ 0,00';
            metaIndividualQuinzenaElement.innerText = 'R$ 0,00';
        }

        if (!isNaN(metaLojaMes) && metaLojaMes > 0) {
            metaLojaMesDisplayElement.innerText = formatCurrency(metaLojaMes);
            metaLojaQuinzenaDisplayElement.innerText = formatCurrency(metaLojaMes / 2);
        } else {
            metaLojaMesDisplayElement.innerText = 'R$ 0,00';
            metaLojaQuinzenaDisplayElement.innerText = 'R$ 0,00';
        }
    }

    function parseCurrency(value) {
        if (!value) return 0;
        return parseFloat(value.replace('R$', '').replace(/\./g, '').replace(',', '.').trim());
    }
    
    function formatCurrency(value) {
        return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(value);
    }

    function setupCurrencyInput(inputId) {
        const input = document.getElementById(inputId);
        if (!input) return;

        input.addEventListener('input', (e) => {
            let value = e.target.value.replace(/\D/g, '');
            if (value) {
                value = (parseInt(value, 10) / 100).toLocaleString('pt-BR', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
                e.target.value = `R$ ${value}`;
            } else {
                e.target.value = '';
            }
        });
    }
    
    ['metaLojaMes', 'vendaLoja1', 'vendaLoja2', 'vendaIndividual1', 'vendaIndividual2'].forEach(setupCurrencyInput);


    function resetCalculator() {
        document.getElementById('grupo').value = '';
        document.getElementById('cargo').value = '';
        document.getElementById('metaLojaMes').value = '';
        document.getElementById('headcount').value = '';
        document.getElementById('vendaLoja1').value = '';
        document.getElementById('vendaLoja2').value = '';
        document.getElementById('vendaIndividual1').value = '';
        document.getElementById('vendaIndividual2').value = '';
        document.getElementById('npsReal').value = '';
        
        document.getElementById('metaIndividualMes').innerText = 'R$ 0,00';
        document.getElementById('metaIndividualQuinzena').innerText = 'R$ 0,00';
        document.getElementById('metaLojaMesDisplay').innerText = 'R$ 0,00';
        document.getElementById('metaLojaQuinzenaDisplay').innerText = 'R$ 0,00';
        document.getElementById('results').innerHTML = '';
        
        document.querySelectorAll('.selection-button').forEach(btn => {
            btn.classList.remove('selected');
        });
        
        document.getElementById('venda-inputs-consultor').style.display = 'none';
        document.getElementById('meta-individual-display').style.display = 'block';
        document.getElementById('meta-gu-display').style.display = 'none';
    }

    function formatPercent(value) {
        return (value * 100).toFixed(2).replace('.', ',') + '%';
    }

    function getAttainmentLevel(attainment, attainmentRules) {
        const sortedRules = [...attainmentRules].sort((a, b) => b.min - a.min);
        for (const rule of sortedRules) {
            if (attainment >= rule.min) {
                return rule.level;
            }
        }
        return "Sem Meta";
    }
    
    const data = {
        npsMeta: 87,
        npsPrize: {
            'Consultora/Especialista': 300,
            'GU': 400
        },
        attainmentRules: [
            { level: "Sem Meta", min: 0 },
            { level: "Meta", min: 1.00 },
            { level: "Super", min: 1.15 },
            { level: "Hiper", min: 1.3 }
        ],
        percentages: [
            // --- DADOS 100% ATUALIZADOS ---
            // GRUPO 1 - Consultora/Especialista
            { cargo: "Consultora/Especialista", group: 1, individual: "Meta", loja: "Sem Meta", rv: 0.0115 },
            { cargo: "Consultora/Especialista", group: 1, individual: "Super", loja: "Sem Meta", rv: 0.0175 },
            { cargo: "Consultora/Especialista", group: 1, individual: "Hiper", loja: "Sem Meta", rv: 0.0290 },
            { cargo: "Consultora/Especialista", group: 1, individual: "Meta", loja: "Meta", rv: 0.0175 },
            { cargo: "Consultora/Especialista", group: 1, individual: "Super", loja: "Meta", rv: 0.0260 },
            { cargo: "Consultora/Especialista", group: 1, individual: "Hiper", loja: "Meta", rv: 0.0430 },
            { cargo: "Consultora/Especialista", group: 1, individual: "Meta", loja: "Super", rv: 0.0200 },
            { cargo: "Consultora/Especialista", group: 1, individual: "Super", loja: "Super", rv: 0.0350 },
            { cargo: "Consultora/Especialista", group: 1, individual: "Hiper", loja: "Super", rv: 0.0475 },
            { cargo: "Consultora/Especialista", group: 1, individual: "Meta", loja: "Hiper", rv: 0.0230 },
            { cargo: "Consultora/Especialista", group: 1, individual: "Super", loja: "Hiper", rv: 0.0375 },
            { cargo: "Consultora/Especialista", group: 1, individual: "Hiper", loja: "Hiper", rv: 0.0575 },
            // GRUPO 1 - GU
            { cargo: "GU", group: 1, loja_quinzena: "Meta", rv: 0.0175 },
            { cargo: "GU", group: 1, loja_quinzena: "Super", rv: 0.0260 },
            { cargo: "GU", group: 1, loja_quinzena: "Hiper", rv: 0.0430 },
            
            // GRUPO 2 - Consultora/Especialista
            { cargo: "Consultora/Especialista", group: 2, individual: "Meta", loja: "Sem Meta", rv: 0.0090 },
            { cargo: "Consultora/Especialista", group: 2, individual: "Super", loja: "Sem Meta", rv: 0.0135 },
            { cargo: "Consultora/Especialista", group: 2, individual: "Hiper", loja: "Sem Meta", rv: 0.0225 },
            { cargo: "Consultora/Especialista", group: 2, individual: "Meta", loja: "Meta", rv: 0.0135 },
            { cargo: "Consultora/Especialista", group: 2, individual: "Super", loja: "Meta", rv: 0.0205 },
            { cargo: "Consultora/Especialista", group: 2, individual: "Hiper", loja: "Meta", rv: 0.0340 },
            { cargo: "Consultora/Especialista", group: 2, individual: "Meta", loja: "Super", rv: 0.0180 },
            { cargo: "Consultora/Especialista", group: 2, individual: "Super", loja: "Super", rv: 0.0270 },
            { cargo: "Consultora/Especialista", group: 2, individual: "Hiper", loja: "Super", rv: 0.0425 },
            { cargo: "Consultora/Especialista", group: 2, individual: "Meta", loja: "Hiper", rv: 0.0210 },
            { cargo: "Consultora/Especialista", group: 2, individual: "Super", loja: "Hiper", rv: 0.0330 },
            { cargo: "Consultora/Especialista", group: 2, individual: "Hiper", loja: "Hiper", rv: 0.0485 },
            // GRUPO 2 - GU
            { cargo: "GU", group: 2, loja_quinzena: "Meta", rv: 0.0135 },
            { cargo: "GU", group: 2, loja_quinzena: "Super", rv: 0.0205 },
            { cargo: "GU", group: 2, loja_quinzena: "Hiper", rv: 0.0340 },

            // GRUPO 3 - Consultora/Especialista
            { cargo: "Consultora/Especialista", group: 3, individual: "Meta", loja: "Sem Meta", rv: 0.0070 },
            { cargo: "Consultora/Especialista", group: 3, individual: "Super", loja: "Sem Meta", rv: 0.0105 },
            { cargo: "Consultora/Especialista", group: 3, individual: "Hiper", loja: "Sem Meta", rv: 0.0175 },
            { cargo: "Consultora/Especialista", group: 3, individual: "Meta", loja: "Meta", rv: 0.0105 },
            { cargo: "Consultora/Especialista", group: 3, individual: "Super", loja: "Meta", rv: 0.0160 },
            { cargo: "Consultora/Especialista", group: 3, individual: "Hiper", loja: "Meta", rv: 0.0265 },
            { cargo: "Consultora/Especialista", group: 3, individual: "Meta", loja: "Super", rv: 0.0140 },
            { cargo: "Consultora/Especialista", group: 3, individual: "Super", loja: "Super", rv: 0.0210 },
            { cargo: "Consultora/Especialista", group: 3, individual: "Hiper", loja: "Super", rv: 0.0350 },
            { cargo: "Consultora/Especialista", group: 3, individual: "Meta", loja: "Hiper", rv: 0.0175 },
            { cargo: "Consultora/Especialista", group: 3, individual: "Super", loja: "Hiper", rv: 0.0260 },
            { cargo: "Consultora/Especialista", group: 3, individual: "Hiper", loja: "Hiper", rv: 0.0400 },
            // GRUPO 3 - GU
            { cargo: "GU", group: 3, loja_quinzena: "Meta", rv: 0.0105 },
            { cargo: "GU", group: 3, loja_quinzena: "Super", rv: 0.0160 },
            { cargo: "GU", group: 3, loja_quinzena: "Hiper", rv: 0.0265 },
            
            // GRUPO 4 - Consultora/Especialista
            { cargo: "Consultora/Especialista", group: 4, individual: "Meta", loja: "Sem Meta", rv: 0.0050 },
            { cargo: "Consultora/Especialista", group: 4, individual: "Super", loja: "Sem Meta", rv: 0.0075 },
            { cargo: "Consultora/Especialista", group: 4, individual: "Hiper", loja: "Sem Meta", rv: 0.0125 },
            { cargo: "Consultora/Especialista", group: 4, individual: "Meta", loja: "Meta", rv: 0.0075 },
            { cargo: "Consultora/Especialista", group: 4, individual: "Super", loja: "Meta", rv: 0.0115 },
            { cargo: "Consultora/Especialista", group: 4, individual: "Hiper", loja: "Meta", rv: 0.0190 },
            { cargo: "Consultora/Especialista", group: 4, individual: "Meta", loja: "Super", rv: 0.0100 },
            { cargo: "Consultora/Especialista", group: 4, individual: "Super", loja: "Super", rv: 0.0150 },
            { cargo: "Consultora/Especialista", group: 4, individual: "Hiper", loja: "Super", rv: 0.0250 },
            { cargo: "Consultora/Especialista", group: 4, individual: "Meta", loja: "Hiper", rv: 0.0125 },
            { cargo: "Consultora/Especialista", group: 4, individual: "Super", loja: "Hiper", rv: 0.0175 },
            { cargo: "Consultora/Especialista", group: 4, individual: "Hiper", loja: "Hiper", rv: 0.0325 },
            // GRUPO 4 - GU
            { cargo: "GU", group: 4, loja_quinzena: "Meta", rv: 0.0075 },
            { cargo: "GU", group: 4, loja_quinzena: "Super", rv: 0.0115 },
            { cargo: "GU", group: 4, loja_quinzena: "Hiper", rv: 0.0190 }
        ]
    };

    document.getElementById('calculateButton').addEventListener('click', calculateRV);

    function calculateRV() {
        const grupo = document.getElementById('grupo').value;
        const cargo = document.getElementById('cargo').value;
        const metaLojaMes = parseCurrency(document.getElementById('metaLojaMes').value);
        const headcount = parseFloat(document.getElementById('headcount').value);
        const vendaLoja1 = parseCurrency(document.getElementById('vendaLoja1').value);
        const vendaLoja2 = parseCurrency(document.getElementById('vendaLoja2').value);
        const vendaIndividual1 = parseCurrency(document.getElementById('vendaIndividual1').value);
        const vendaIndividual2 = parseCurrency(document.getElementById('vendaIndividual2').value);
        const npsReal = parseFloat(document.getElementById('npsReal').value);
        
        if (!grupo || !cargo || isNaN(metaLojaMes) || metaLojaMes <= 0 || isNaN(headcount) || headcount <= 0 || isNaN(vendaLoja1) || isNaN(vendaLoja2) || (cargo === 'Consultora/Especialista' && (isNaN(vendaIndividual1) || isNaN(vendaIndividual2))) || isNaN(npsReal) ) {
            alert("Por favor, preencha todos os campos obrigatórios com valores válidos. Meta e Headcount devem ser maiores que zero.");
            return;
        }

        const metaLojaQuinzena = metaLojaMes / 2;
        const vendaLojaTotal = vendaLoja1 + vendaLoja2;
        const attainmentLojaMes = vendaLojaTotal / metaLojaMes;
        const nivelAtingimentoLojaMes = getAttainmentLevel(attainmentLojaMes, data.attainmentRules);

        const metaIndividualQuinzena = metaLojaMes / headcount / 2;
        
        let rvTotal = 0, rv1 = 0, rv2 = 0, rvPercentage1 = 0, rvPercentage2 = 0;
        let attainmentIndividual1, nivelAtingimentoIndividual1, attainmentIndividual2, nivelAtingimentoIndividual2;

        if (cargo === "Consultora/Especialista") {
            attainmentIndividual1 = metaIndividualQuinzena > 0 ? vendaIndividual1 / metaIndividualQuinzena : 0;
            nivelAtingimentoIndividual1 = getAttainmentLevel(attainmentIndividual1, data.attainmentRules);
            
            attainmentIndividual2 = metaIndividualQuinzena > 0 ? vendaIndividual2 / metaIndividualQuinzena : 0;
            nivelAtingimentoIndividual2 = getAttainmentLevel(attainmentIndividual2, data.attainmentRules);

            const applicablePercentage1 = data.percentages.find(p => p.cargo === cargo && parseInt(p.group) === parseInt(grupo) && p.individual === nivelAtingimentoIndividual1 && p.loja === nivelAtingimentoLojaMes);
            if (applicablePercentage1) {
                rvPercentage1 = applicablePercentage1.rv;
                rv1 = vendaIndividual1 * rvPercentage1;
            }

            const applicablePercentage2 = data.percentages.find(p => p.cargo === cargo && parseInt(p.group) === parseInt(grupo) && p.individual === nivelAtingimentoIndividual2 && p.loja === nivelAtingimentoLojaMes);
            if (applicablePercentage2) {
                rvPercentage2 = applicablePercentage2.rv;
                rv2 = vendaIndividual2 * rvPercentage2;
            }
            rvTotal = rv1 + rv2;

        } else if (cargo === "GU") {
            const attainmentLoja1 = metaLojaQuinzena > 0 ? vendaLoja1 / metaLojaQuinzena : 0;
            nivelAtingimentoIndividual1 = getAttainmentLevel(attainmentLoja1, data.attainmentRules); 

            const attainmentLoja2 = metaLojaQuinzena > 0 ? vendaLoja2 / metaLojaQuinzena : 0;
            nivelAtingimentoIndividual2 = getAttainmentLevel(attainmentLoja2, data.attainmentRules); 

            const applicablePercentage1 = data.percentages.find(p => p.cargo === cargo && parseInt(p.group) === parseInt(grupo) && p.loja_quinzena === nivelAtingimentoIndividual1);
            if (applicablePercentage1) {
                rvPercentage1 = applicablePercentage1.rv;
                rv1 = vendaLoja1 * rvPercentage1;
            }

            const applicablePercentage2 = data.percentages.find(p => p.cargo === cargo && parseInt(p.group) === parseInt(grupo) && p.loja_quinzena === nivelAtingimentoIndividual2);
            if (applicablePercentage2) {
                rvPercentage2 = applicablePercentage2.rv;
                rv2 = vendaLoja2 * rvPercentage2;
            }
            rvTotal = rv1 + rv2;
            attainmentIndividual1 = attainmentLoja1;
            attainmentIndividual2 = attainmentLoja2;
        }

        const npsPrize = (attainmentLojaMes >= 1.0 && npsReal >= data.npsMeta) ? data.npsPrize[cargo] : 0;
        const remuneracaoTotal = rvTotal + npsPrize;

        displayResults({ cargo, grupo, metaLojaMes, vendaLojaTotal, attainmentLojaMes, nivelAtingimentoLojaMes, attainmentIndividual1, nivelAtingimentoIndividual1, rvPercentage1, rv1, attainmentIndividual2, nivelAtingimentoIndividual2, rvPercentage2, rv2, rvTotal, npsPrize, remuneracaoTotal });
    }

    function displayResults(results) {
        const resultsDiv = document.getElementById('results');
        resultsDiv.innerHTML = '';
        
        let gridHTML = '', descriptiveHTML = '';

        if (results.cargo === 'Consultora/Especialista') {
            const lojaAttainmentLevels = ["Sem Meta", "Meta", "Super", "Hiper"];
            const individualAttainmentLevels = ["Meta", "Super", "Hiper"];
            
             descriptiveHTML = `
                <div class="info-box">
                    Na <strong>1ª quinzena</strong>, com o resultado da <strong>loja no mês</strong> em <strong>${results.nivelAtingimentoLojaMes}</strong> (${formatPercent(results.attainmentLojaMes)}), sua linha de ganhos foi definida. Seu resultado <strong>individual nesta quinzena</strong> foi <strong>${results.nivelAtingimentoIndividual1}</strong> (${formatPercent(results.attainmentIndividual1)}), definindo sua coluna. Isso resultou em um RV de <strong>${formatPercent(results.rvPercentage1)}</strong>.
                </div>
                <div class="info-box">
                    Na <strong>2ª quinzena</strong>, a linha de ganhos (<strong>${results.nivelAtingimentoLojaMes}</strong> da loja) se manteve, mas seu resultado <strong>individual</strong> foi <strong>${results.nivelAtingimentoIndividual2}</strong> (${formatPercent(results.attainmentIndividual2)}), alterando sua coluna. Isso resultou em um RV de <strong>${formatPercent(results.rvPercentage2)}</strong>.
                </div>`;

            const grid1 = `<div class="grid-container"><h4>Matriz de Ganhos - 1ª Quinzena</h4><table class="rv-grid"><thead><tr><th>Loja(Mês)</th>${individualAttainmentLevels.map(l=>`<th>${l}(Ind)</th>`).join('')}</tr></thead><tbody>${lojaAttainmentLevels.map(lojaLvl => `<tr><td class="axis-y">${lojaLvl}</td>${individualAttainmentLevels.map(indLvl => {const p = data.percentages.find(p => p.cargo === results.cargo && parseInt(p.group) === parseInt(results.grupo) && p.loja === lojaLvl && p.individual === indLvl); const rv = p ? formatPercent(p.rv) : '-'; const highlight = lojaLvl === results.nivelAtingimentoLojaMes && indLvl === results.nivelAtingimentoIndividual1 ? 'highlight' : ''; return `<td class="${highlight}">${rv}</td>`;}).join('')}</tr>`).join('')}</tbody></table></div>`;
            const grid2 = `<div class="grid-container"><h4>Matriz de Ganhos - 2ª Quinzena</h4><table class="rv-grid"><thead><tr><th>Loja(Mês)</th>${individualAttainmentLevels.map(l=>`<th>${l}(Ind)</th>`).join('')}</tr></thead><tbody>${lojaAttainmentLevels.map(lojaLvl => `<tr><td class="axis-y">${lojaLvl}</td>${individualAttainmentLevels.map(indLvl => {const p = data.percentages.find(p => p.cargo === results.cargo && parseInt(p.group) === parseInt(results.grupo) && p.loja === lojaLvl && p.individual === indLvl); const rv = p ? formatPercent(p.rv) : '-'; const highlight = lojaLvl === results.nivelAtingimentoLojaMes && indLvl === results.nivelAtingimentoIndividual2 ? 'highlight' : ''; return `<td class="${highlight}">${rv}</td>`;}).join('')}</tr>`).join('')}</tbody></table></div>`;
            gridHTML = grid1 + grid2;

        } else if (results.cargo === 'GU') {
            const guAttainmentLevels = ["Meta", "Super", "Hiper"];

            descriptiveHTML = `
                <div class="info-box">
                    Na <strong>1ª quinzena</strong>, o resultado da <strong>loja na quinzena</strong> foi <strong>${results.nivelAtingimentoIndividual1}</strong> (${formatPercent(results.attainmentIndividual1)}), resultando em um RV de <strong>${formatPercent(results.rvPercentage1)}</strong>.
                </div>
                <div class="info-box">
                    Na <strong>2ª quinzena</strong>, o resultado da <strong>loja na quinzena</strong> foi <strong>${results.nivelAtingimentoIndividual2}</strong> (${formatPercent(results.attainmentIndividual2)}), resultando em um RV de <strong>${formatPercent(results.rvPercentage2)}</strong>.
                </div>`;

            const grid1 = `<div class="grid-container"><h4>Matriz de Ganhos - 1ª Quinzena (GU)</h4><table class="rv-grid"><thead><tr>${guAttainmentLevels.map(l => `<th>${l} (Loja)</th>`).join('')}</tr></thead><tbody><tr>${guAttainmentLevels.map(level => { const p = data.percentages.find(p => p.cargo === results.cargo && parseInt(p.group) === parseInt(results.grupo) && p.loja_quinzena === level); const rv = p ? formatPercent(p.rv) : '-'; const highlight = level === results.nivelAtingimentoIndividual1 ? 'highlight' : ''; return `<td class="${highlight}">${rv}</td>`; }).join('')}</tr></tbody></table></div>`;
            const grid2 = `<div class="grid-container"><h4>Matriz de Ganhos - 2ª Quinzena (GU)</h4><table class="rv-grid"><thead><tr>${guAttainmentLevels.map(l => `<th>${l} (Loja)</th>`).join('')}</tr></thead><tbody><tr>${guAttainmentLevels.map(level => { const p = data.percentages.find(p => p.cargo === results.cargo && parseInt(p.group) === parseInt(results.grupo) && p.loja_quinzena === level); const rv = p ? formatPercent(p.rv) : '-'; const highlight = level === results.nivelAtingimentoIndividual2 ? 'highlight' : ''; return `<td class="${highlight}">${rv}</td>`; }).join('')}</tr></tbody></table></div>`;
            gridHTML = grid1 + grid2;
        }
        
        const reportHTML = `
            <div class="report-section">
                <h3>Extrato de Cálculo Detalhado</h3>
                <div class="result-item"><span class="result-label">Atingimento Total da Loja (Mês):</span><span class="result-value">${formatPercent(results.attainmentLojaMes)} (${results.nivelAtingimentoLojaMes})</span></div>
                
                ${descriptiveHTML}

                ${gridHTML}

                <div class="report-section">
                    <h4>Resumo da Remuneração</h4>
                    <div class="result-item"><span class="result-label">Valor RV (1ª Quinzena):</span><span class="result-value">${formatCurrency(results.rv1)}</span></div>
                    <div class="result-item"><span class="result-label">Valor RV (2ª Quinzena):</span><span class="result-value">${formatCurrency(results.rv2)}</span></div>
                    <div class="result-item"><span class="result-label"><b>RV Total (Soma Quinzenas):</b></span><span class="result-value"><b>${formatCurrency(results.rvTotal)}</b></span></div>
                    <div class="result-item"><span class="result-label">Prêmio NPS:</span><span class="result-value">${formatCurrency(results.npsPrize)}</span></div>
                    <div class="result-item"><span class="result-label final-rv">Remuneração Total:</span><span class="result-value final-rv">${formatCurrency(results.remuneracaoTotal)}</span></div>
                </div>
            </div>
            <div class="button-group">
                <button id="resetButton">Refazer Cálculo</button>
            </div>
        `;
        resultsDiv.innerHTML = reportHTML;
        document.getElementById('resetButton').addEventListener('click', resetCalculator);
    }
</script>
</body>
</html>
