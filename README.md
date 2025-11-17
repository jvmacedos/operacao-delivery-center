# 📦 Delivery Center – Business Intelligence Dashboard
## Power BI • Modelagem em Estrela • DAX • UX/UI • Análise Comercial, Logística e Geográfica

# 📌 Descrição Geral do Projeto
Este projeto foi desenvolvido com o objetivo de consolidar e analisar dados de operação, demanda, logística e geografia em um ecossistema de delivery.
O dashboard integra diversas fontes de dados e transforma informações brutas em insights estratégicos.
  Ele foi construído utilizando:
    - Power BI (com DAX e Power Query)
    - Modelagem Dimensional (Star Schema)
    - Design customizado no Canva
    - Google Drive para armazenamento dos dados

O resultado final é um painel completo que permite monitorar performance comercial, eficiência logística e comportamento geográfico, além de avaliar a qualidade da operação.

# 🧠 Objetivos do Dashboard
- Monitorar as principais métricas do negócio
- Analisar o desempenho comercial por loja e por canal
- Avaliar a eficiência logística e o SLA de entrega
- Identificar gargalos e oportunidades
- Fornecer insights espaciais sobre demanda e operação
- Auxiliar equipes de gestão na tomada de decisão baseada em dados

# 🛠 Processo Completo do Projeto
## 1️⃣ **Extração dos Dados**
  Os dados foram disponibilizados em múltiplas tabelas CSV:
    - Pedidos
    - Hubs
    - Lojas
    - Entregas
    - Motoristas
    - Pagamentos
    - Canais

Todos os arquivos foram armazenados em:

📂 Google Drive
Para permitir atualização automática no Power BI, sem necessidade de permissão adicional para avaliadores.

## 2️⃣ **Tratamento e Limpeza (Power Query)**
  As principais transformações foram:

  ✔ Padronização:
    ✔ Renomeação de colunas
    ✔ Conversão de tipos (inteiro, decimal, data, texto) 
    ✔ Nomes de cidade e loja normalizados 
    ✔ Remoção de acentos
    
  ✔ Correções importantes:
    ✔ Criação de bins de tempo de entrega 
    ✔ Limpeza de valores vazios ou duplicados 
    ✔ Conversão de distância em metros → quilômetros 
    ✔ Cálculo de tempo de entrega sem outliers

  ✔ Construção das Dimensões:
    ✔ Dim_Date 
    ✔ Dim_Store 
    ✔ Dim_Channel 
    ✔ Dim_Hub 
    ✔ Dim_City

## 3️⃣ **Modelagem de Dados (Star Schema)**
O modelo estrela foi adotado para fazer o relacionamento das tabelas dimensão para a tabela fato.

Regras aplicadas:
  - Relacionamentos 1:N
  - Calendário contínuo
  - Campos derivados para hierarquias de data
  - Ordenação de DayOfWeekName por DayOfWeekNumber
## 4️⃣ **Criação de Métricas DAX**

Canais Ativos: 
		Canais Ativos = DISTINCTCOUNT(Dim_Channel[channel_name])
    
Cidade Selecionada:
		Cidade Selecionada = VAR Cidade = SELECTEDVALUE( Dim_Hub[hub_city], "Todas as Cidades" )
RETURN Cidade

Cidades:
	Cidades = DISTINCTCOUNT( Dim_Hub[hub_city] )
  
Distância média de entrega (min):
	Distância mêdia de entrega(km) = AVERAGE(F_Orders[delivery_distance_meters]) / 1000
  
Lojas ativas:
	Lojas ativas = DISTINCTCOUNT ( Dim_Store[store_id] )
  
Média de Custo do Delivery:
	Mediana do tempo de entrega (min) = 
PERCENTILEX.INC (
    FILTER (
      		  F_Orders,
       		 NOT ISBLANK ( F_Orders[delivery_time_minutes] )
          		 && F_Orders[order_status] = "Finished"
   		 ),
    	F_Orders[delivery_time_minutes],
    	0.5
)

Pedidos cancelados:
		Pedidos cancelados = 
CALCULATE ( [Total de Pedidos], F_Orders[order_status] = "CANCELED")

Pedidos cancelados (%):
		Pedidos Cancelados % = 
DIVIDE ( [Pedidos cancelados], [Total de Pedidos] )

Pedidos no prazo:
		Pedidos no prazo = 
CALCULATE (
    [Total de Pedidos],
    F_Orders[is_on_time] = 1
)

Pedidos no prazo (%):
	Pedidos no prazo % = 
DIVIDE ( [Pedidos no prazo], [Total de Pedidos] )

Pedidos por centro de Distribuição:
	Pedidos por Centro de Distribuição = CALCULATE( [Total de Pedidos], VALUES(Dim_Hub[hub_id]) )
  
Quantidade de centros de distribuição:
	Quantidade de Centros de Distribuição = DISTINCTCOUNT( Dim_Hub[hub_id] )
  
Receita total:
	Quantidade de Centros de Distribuição = DISTINCTCOUNT( Dim_Hub[hub_id] )
  
Tempo médio de entrega:
	Tempo médio de entrega (min) = AVERAGE ( F_Orders[delivery_time_minutes] )
  
Ticket médio:
	Ticket Médio = DIVIDE ( [Receita total], [Total de Pedidos] )
  
Total de pedidos:
	Total de Pedidos = COUNTROWS ( F_Orders )

## 5️⃣ **Design e UX/UI**
O design foi inteiramente construído no Canva e recriado no Power BI usando:

✔ Elementos personalizados:
    - HUD superior
    - Menu lateral com navegação
    - Ícones para cada página
    - Paleta de cores corporativa
✔ Navigation
Botões de navegação configurados página a página, com estado ativo (laranja) e inativo (cinza).

✔ Tooltip
Foi criado um tooltip avançado para o mapa, exibindo:
- Cidade
- Total de pedidos
- Receita
- Lojas ativas

# 📊 TELAS DO DASHBOARD
## 1️⃣ Tela – Visão Geral
![WhatsApp Image 2025-11-17 at 11 24 18](https://github.com/user-attachments/assets/4f679342-6f19-4a61-9615-2850a4a6f58b)

🎯 Intenção da Tela
Fornecer uma visão executiva da operação, permitindo que o gestor entenda imediatamente o estado do negócio.

KPI’s:
- Total de Pedidos
- Receita Total
- Ticket Médio
- % Pedidos no Prazo
- % Cancelamentos
- Tempo Mediano de Entrega

Gráficos:
- Pedidos por Cidade
- Pedidos por Canal
- Pedidos e Receita por Mês

## 2️⃣ Tela – Comercial & Demanda
![WhatsApp Image 2025-11-17 at 11 24 18 (1)](https://github.com/user-attachments/assets/bf547944-2e64-43bc-9b5d-d9482ea7c392)

🎯 Intenção da Tela

Avaliar os padrões de compra, comportamento dos clientes e desempenho comercial das lojas.

KPI’s:
- Total de Pedidos
- Canais ativos
- Lojas Ativas
- Cidades atendidas

Gráficos:
- Pedidos por Canal
- Pedidos por Dia da Semana
- Pedidos por Loja
- Mapa de Pedidos por Cidade

## 3️⃣ Tela – Operação & Performance Logística
![WhatsApp Image 2025-11-17 at 11 24 19](https://github.com/user-attachments/assets/85bbc912-8272-4e04-bebe-8595e985bf97)

🎯 Intenção da Tela

Avaliar a qualidade e a velocidade das entregas, analisando performance por hub e modal.

KPI’s:
- Mediana do Tempo de Entrega
- % de Pedidos no Prazo
- Distância Média
- Custo Médio
- % de pedidos cancelados

Gráficos:
- Tempo Médio por Hub
- % no Prazo por Hub
- Pedidos por Modal
- Histograma de Tempo de Entrega

## 4️⃣ Tela – Análise Geográfica
![WhatsApp Image 2025-11-17 at 11 24 19 (1)](https://github.com/user-attachments/assets/278eeaa4-1f64-4e4f-ba37-909b74168f79)

🎯 Intenção da Tela

Visualizar a operação no território, entendendo concentração de hubs, lojas e cidades atendidas.

KPI’s:
- Quantidade de centros de distribuição (hubs)
- Lojas Ativas
- Cidades

Gráficos:
- Mapa de Pedidos por Cidade
- Pedidos por Loja
- Tabela de Hubs e Lojas

# 🏁 Conclusão

O projeto consolida o ciclo completo de Business Intelligence:
		📌 Coleta → 📌 Tratamento → 📌 Modelagem → 📌 DAX → 📌 Design → 📌 Análise → 📌 Documentação
Ele demonstra competências técnicas em:
- Power BI
- Modelagem dimensional
- DAX avançado
- ETL
- UX para dashboards
- Storytelling com dados

E entrega uma visão detalhada da operação de delivery, permitindo tomadas de decisões rápidas e assertivas.
