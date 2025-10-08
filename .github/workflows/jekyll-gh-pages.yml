<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Consumo da Moto - km/l</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --bg: #121212;
            --card-bg: #1e1e1e;
            --text: #ffffff;
            --text-secondary: #b0b0b0;
            --accent: #4CAF50;
            --border: #333;
            --hover: #45a049;
        }
        [data-theme="light"] {
            --bg: #f4f4f4;
            --card-bg: #ffffff;
            --text: #333333;
            --text-secondary: #666666;
            --accent: #4CAF50;
            --border: #ddd;
            --hover: #45a049;
        }
        body { 
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; 
            margin: 0; padding: 0; 
            background: var(--bg); 
            color: var(--text); 
            min-height: 100vh; 
        }
        .container { max-width: 100%; margin: 0; padding: 20px; }
        h1 { text-align: center; margin: 20px 0; font-size: 1.8em; color: var(--text); }
        .card { background: var(--card-bg); padding: 20px; border-radius: 12px; box-shadow: 0 4px 12px rgba(0,0,0,0.2); margin-bottom: 20px; border: 1px solid var(--border); }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-weight: 600; color: var(--text-secondary); font-size: 0.9em; }
        input, select { width: 100%; padding: 12px; border: 1px solid var(--border); border-radius: 8px; background: var(--bg); color: var(--text); font-size: 1em; box-sizing: border-box; }
        input:focus, select:focus { outline: none; border-color: var(--accent); }
        button { width: 100%; padding: 12px; background: var(--accent); color: white; border: none; border-radius: 8px; font-size: 1em; cursor: pointer; font-weight: 600; margin-bottom: 10px; }
        button:hover { background: var(--hover); }
        .btn-reset { background: #f44336; }
        .btn-reset:hover { background: #d32f2f; }
        .btn-small { width: auto; padding: 8px 16px; margin: 5px; }
        .lista { max-height: 300px; overflow-y: auto; }
        .item { padding: 12px; border-bottom: 1px solid var(--border); display: flex; justify-content: space-between; align-items: center; font-size: 0.9em; }
        .item:last-child { border-bottom: none; }
        .resultados { display: grid; gap: 15px; }
        .media { font-weight: bold; color: var(--accent); font-size: 1.2em; }
        .dias-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(80px, 1fr)); gap: 10px; }
        .dia-semana { text-align: center; padding: 8px; background: rgba(255,255,255,0.05); border-radius: 6px; }
        .chart-container { position: relative; height: 250px; margin: 20px 0; }
        .modo-section { background: rgba(255,255,255,0.05); padding: 10px; border-radius: 6px; margin: 10px 0; }
        .theme-toggle { position: fixed; bottom: 20px; right: 20px; width: 50px; height: 50px; background: var(--card-bg); color: var(--text); border: none; border-radius: 50%; font-size: 1.5em; cursor: pointer; box-shadow: 0 4px 12px rgba(0,0,0,0.3); z-index: 1000; }
        .theme-toggle:hover { transform: scale(1.1); }
        @media (min-width: 600px) { .container { max-width: 500px; margin: 0 auto; } }
    </style>
</head>
<body data-theme="dark">
    <div class="container">
        <h1>Consumo da Moto</h1>
        
        <div class="card">
            <button type="button" class="btn-reset" onclick="zerarDados()">🗑️ Zerar Todos os Dados</button>
            <form id="formAbastecimento">
                <div class="form-group">
                    <label>Data do abastecimento:</label>
                    <input type="date" id="data" required>
                </div>
                <div class="form-group">
                    <label>Modo de uso:</label>
                    <select id="modo" required>
                        <option value="cidade">Cidade</option>
                        <option value="estrada">Estrada</option>
                        <option value="viagem">Viagem</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Km atual (odômetro):</label>
                    <input type="number" id="km" step="0.01" placeholder="Ex: 1250.5" required>
                </div>
                <div class="form-group">
                    <label>Litros abastecidos:</label>
                    <input type="number" id="litros" step="0.01" placeholder="Ex: 10.5" required>
                </div>
                <button type="submit">Adicionar Abastecimento</button>
            </form>
        </div>

        <!-- Configurações para Previsão -->
        <div class="card">
            <h3>Configurações para Previsão</h3>
            <div class="form-group">
                <label>Tamanho do tanque (L):</label>
                <input type="number" id="tanque" step="0.01" placeholder="Ex: 15" value="15">
            </div>
            <div class="form-group">
                <label>Preço da gasolina (R$/L):</label>
                <input type="number" id="preco" step="0.01" placeholder="Ex: 5.50" value="5.50">
            </div>
            <button type="button" class="btn-small" onclick="salvarConfig()">Salvar Configs</button>
        </div>
        
        <div class="card">
            <h3>Abastecimentos Registrados</h3>
            <div class="lista" id="itensLista"></div>
        </div>
        
        <div class="card">
            <div class="resultados">
                <h3>Informações Complementares</h3>
                <p>Média geral km/l: <span id="mediaGeral" class="media">0</span></p>
                <div id="previsao"></div>
                <h4>Abastecimentos por dia da semana:</h4>
                <div class="dias-grid" id="diasSemana"></div>
                <h4>Média de litros gastos por dia da semana:</h4>
                <div class="dias-grid" id="mediaDias"></div>
                <div id="mediasModo"></div>
            </div>
        </div>

        <!-- Gráfico -->
        <div class="card">
            <h3>Evolução do Consumo (km/l)</h3>
            <div class="chart-container">
                <canvas id="chartConsumo"></canvas>
            </div>
        </div>

        <!-- Export/Import -->
        <div class="card">
            <h3>Exportar/Importar Dados</h3>
            <button type="button" onclick="exportarDados()">📥 Exportar CSV</button>
            <input type="file" id="fileImport" accept=".csv" style="display: none;" onchange="importarDados(event)">
            <button type="button" onclick="document.getElementById('fileImport').click()">📤 Importar CSV</button>
        </div>
    </div>

    <button class="theme-toggle" id="themeToggle" onclick="toggleTheme()">🌙</button>

    <script>
        const form = document.getElementById('formAbastecimento');
        const listaItens = document.getElementById('itensLista');
        const mediaGeralEl = document.getElementById('mediaGeral');
        const diasSemanaEl = document.getElementById('diasSemana');
        const mediaDiasEl = document.getElementById('mediaDias');
        const mediasModoEl = document.getElementById('mediasModo');
        const previsaoEl = document.getElementById('previsao');
        const themeToggle = document.getElementById('themeToggle');
        const body = document.body;
        const ctx = document.getElementById('chartConsumo').getContext('2d');
        let chart;

        let abastecimentos = JSON.parse(localStorage.getItem('abastecimentosMoto')) || [];
        let config = JSON.parse(localStorage.getItem('configMoto')) || { tanque: 15, preco: 5.50 };
        let temaAtual = localStorage.getItem('temaMoto') || 'dark';

        // Carrega tema
        body.setAttribute('data-theme', temaAtual);
        themeToggle.textContent = temaAtual === 'dark' ? '☀️' : '🌙';

        // Carrega configs
        document.getElementById('tanque').value = config.tanque;
        document.getElementById('preco').value = config.preco;

        function salvarDados() {
            localStorage.setItem('abastecimentosMoto', JSON.stringify(abastecimentos));
        }

        function salvarConfig() {
            config.tanque = parseFloat(document.getElementById('tanque').value);
            config.preco = parseFloat(document.getElementById('preco').value);
            localStorage.setItem('configMoto', JSON.stringify(config));
            calcularMedias(); // Recalcula previsão
        }

        function salvarTema() {
            localStorage.setItem('temaMoto', temaAtual);
        }

        function toggleTheme() {
            temaAtual = temaAtual === 'dark' ? 'light' : 'dark';
            body.setAttribute('data-theme', temaAtual);
            themeToggle.textContent = temaAtual === 'dark' ? '☀️' : '🌙';
            salvarTema();
        }

        function zerarDados() {
            if (confirm('Tem certeza? Isso vai apagar todos os abastecimentos!')) {
                abastecimentos = [];
                salvarDados();
                renderizarLista();
                calcularMedias();
                renderizarGrafico();
            }
        }

        function renderizarLista() {
            listaItens.innerHTML = '';
            abastecimentos.forEach((item, index) => {
                const div = document.createElement('div');
                div.className = 'item';
                const dataFormatada = new Date(item.data).toLocaleDateString('pt-BR', {weekday: 'short', day: '2-digit', month: 'short'});
                const kmLIntervalo = index > 0 ? ((item.km - abastecimentos[index-1].km) / item.litros).toFixed(2) : '-';
                div.innerHTML = `
                    <span>${dataFormatada} - ${item.modo} - Km: ${item.km} | Litros: ${item.litros}</span>
                    <span>${kmLIntervalo} km/l</span>
                `;
                listaItens.appendChild(div);
            });
        }

        function calcularMedias() {
            if (abastecimentos.length < 2) {
                mediaGeralEl.textContent = '0';
                previsaoEl.innerHTML = '';
                return;
            }

            let totalKmRodados = 0;
            let totalLitros = 0;
            for (let i = 1; i < abastecimentos.length; i++) {
                const kmRodados = abastecimentos[i].km - abastecimentos[i-1].km;
                totalKmRodados += kmRodados;
                totalLitros += abastecimentos[i].litros;
            }
            const media = (totalKmRodados / totalLitros).toFixed(2);
            mediaGeralEl.textContent = media;

            // Previsão de autonomia
            const autonomia = (media * config.tanque).toFixed(0);
            const custoPorKm = (config.preco / media).toFixed(2);
            previsaoEl.innerHTML = `
                <p>Autonomia com tanque cheio: <span class="media">${autonomia} km</span></p>
                <p>Custo por km: R$ <span class="media">${custoPorKm}</span></p>
            `;

            // Contar por dia da semana
            const dias = {0: 0, 1: 0, 2: 0, 3: 0, 4: 0, 5: 0, 6: 0};
            const totalLitrosPorDia = {0: 0, 1: 0, 2: 0, 3: 0, 4: 0, 5: 0, 6: 0};
            abastecimentos.forEach(item => {
                const dia = new Date(item.data).getDay();
                dias[dia]++;
                totalLitrosPorDia[dia] += item.litros;
            });

            // Renderizar contagem
            diasSemanaEl.innerHTML = '';
            Object.keys(dias).forEach(dia => {
                const nome = ['Dom', 'Seg', 'Ter', 'Qua', 'Qui', 'Sex', 'Sáb'][dia];
                const div = document.createElement('div');
                div.className = 'dia-semana';
                div.innerHTML = `<strong>${nome}:</strong> ${dias[dia]}`;
                diasSemanaEl.appendChild(div);
            });

            // Renderizar média litros
            mediaDiasEl.innerHTML = '';
            Object.keys(totalLitrosPorDia).forEach(dia => {
                const nome = ['Dom', 'Seg', 'Ter', 'Qua', 'Qui', 'Sex', 'Sáb'][dia];
                const mediaLitros = dias[dia] > 0 ? (totalLitrosPorDia[dia] / dias[dia]).toFixed(2) : '0';
                const div = document.createElement('div');
                div.className = 'dia-semana';
                div.innerHTML = `<strong>${nome}:</strong> ${mediaLitros} L`;
                mediaDiasEl.appendChild(div);
            });

            // Médias por modo
            const modulosUnicos = [...new Set(abastecimentos.map(a => a.modo))];
            mediasModoEl.innerHTML = '<h4>Médias por Modo:</h4>';
            modulosUnicos.forEach(modo => {
                const filtrados = abastecimentos.filter(a => a.modo === modo);
                if (filtrados.length < 2) return;
                let totalKmM = 0;
                let totalLitrosM = 0;
                for (let i = 1; i < filtrados.length; i++) {
                    const kmRodados = filtrados[i].km - filtrados[i-1].km;
                    totalKmM += kmRodados;
                    totalLitrosM += filtrados[i].litros;
                }
                const mediaM = (totalKmM / totalLitrosM).toFixed(2);
                const div = document.createElement('div');
                div.className = 'modo-section';
                div.innerHTML = `<strong>${modo}:</strong> ${mediaM} km/l (${filtrados.length} regs.)`;
                mediasModoEl.appendChild(div);
            });
        }

        function renderizarGrafico() {
            const labels = abastecimentos.map((item, index) => index > 0 ? new Date(item.data).toLocaleDateString('pt-BR') : '');
            const data = [];
            for (let i = 1; i < abastecimentos.length; i++) {
                data.push(((abastecimentos[i].km - abastecimentos[i-1].km) / abastecimentos[i].litros).toFixed(2));
            }

            if (chart) chart.destroy();
            chart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels.slice(1),
                    datasets: [{
                        label: 'km/l por abastecimento',
                        data: data,
                        borderColor: 'rgb(76, 175, 80)',
                        backgroundColor: 'rgba(76, 175, 80, 0.2)',
                        tension: 0.1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: { beginAtZero: true }
                    }
                }
            });
        }

        function exportarDados() {
            if (abastecimentos.length === 0) return alert('Nenhum dado para exportar!');
            const csv = ['Data,Modo,Km,Litros\n'] + abastecimentos.map(a => `${a.data},${a.modo},${a.km},${a.litros}`).join('\n');
            const blob = new Blob([csv], { type: 'text/csv' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'abastecimentos-moto.csv';
            a.click();
        }

        function importarDados(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = (e) => {
                const text = e.target.result;
                const lines = text.split('\n').slice(1); // Skip header
                const novos = lines.map(line => {
                    const [data, modo, km, litros] = line.split(',');
                    return { data, modo, km: parseFloat(km), litros: parseFloat(litros) };
                }).filter(a => !isNaN(a.km) && !isNaN(a.litros));
                if (novos.length > 0) {
                    abastecimentos = [...abastecimentos, ...novos];
                    abastecimentos.sort((a, b) => new Date(a.data) - new Date(b.data));
                    salvarDados();
                    renderizarLista();
                    calcularMedias();
                    renderizarGrafico();
                    alert(`${novos.length} registros importados!`);
                }
            };
            reader.readAsText(file);
        }

        form.addEventListener('submit', (e) => {
            e.preventDefault();
            const data = document.getElementById('data').value;
            const modo = document.getElementById('modo').value;
            const km = parseFloat(document.getElementById('km').value);
            const litros = parseFloat(document.getElementById('litros').value);

            if (isNaN(km) || isNaN(litros) || !data || !modo) return alert('Preencha todos os campos!');

            abastecimentos.push({ data, modo, km, litros });
            abastecimentos.sort((a, b) => new Date(a.data) - new Date(b.data));
            salvarDados();
            renderizarLista();
            calcularMedias();
            renderizarGrafico();

            form.reset();
            document.getElementById('data').focus();
            document.getElementById('modo').value = 'cidade'; // Default
        });

        // Carrega dados iniciais
        renderizarLista();
        calcularMedias();
        renderizarGrafico();

        // Data atual como default
        document.getElementById('data').valueAsDate = new Date();
    </script>
</body>
</html>
