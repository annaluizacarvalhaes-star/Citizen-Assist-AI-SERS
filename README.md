# Documentação Técnica: Módulo de Energia e Sustentabilidade — Citizen Assist AI

Materia: 

Integrantes:

Anna Luiza Carvalhaes RM: 573330

Kethelyn de Oliveira Rocha RM: 574016


---

## 1. Definição do Contexto

* **Público Escolhido:** Cidadãos idosos (60 anos ou mais) dependentes de renda fixa (aposentadoria ou BPC) e usuários do Sistema Único de Saúde (SUS).
* **Necessidade Energética e Orçamentária Abordada:** O alto impacto financeiro da conta de energia elétrica residencial (climatização, equipamentos médicos domiciliares ou eletrodomésticos antigos) somado aos gastos recorrentes com saúde e medicamentos básicos, o que compromete o orçamento mensal do idoso.
* **Contribuição das Orientações:** O assistente digital permite que o idoso ou seu cuidador simule, em tempo real e em linguagem acessível, o gasto mensal com energia e saúde, orientando sobre o acesso gratuito a benefícios públicos (**Tarifa Social de Energia Elétrica** e **Farmácia Popular do SUS**) e sobre a adoção de **energia solar fotovoltaica** como alternativa sustentável de longo prazo.

---

## 2. Análise de Consumo e Premissas

A análise implementada no código base utiliza parâmetros padronizados para refletir a realidade de uma residência típica de idosos de baixa ou média renda. Todos os dados estimados estão explicitados nas variáveis do algoritmo:

* **Consumo Residencial de Referência (Dado Estimado):** Consumo mensal médio de **$E = 150,0\text{ kWh/mês}$**, considerando eletrodomésticos essenciais e equipamentos básicos de conforto térmico/saúde.
* **Período Considerado:** Ciclo mensal padrão de **$30\text{ dias}$**.
* **Tarifa de Energia Convencional (Dado Estimado):** Valor médio de **$R\$\ 0,85/\text{kWh}$** (`tarifa_kwh = 0.85`), representando a tarifa residencial convencional (Grupo B1) com impostos inclusos.
* **Origem dos Dados:** Parâmetros baseados nas tabelas indicativas de consumo do **PROCEL / Eletrobras**, diretrizes tarifárias da **ANEEL** e regras do **Ministério da Saúde / CadÚnico**.

### Fórmulas Aplicadas no Código

1. **Custo Mensal Convencional ($C_{\text{atual}}$):**
$$C_{\text{atual}} = E_{\text{mensal}} \times \text{Tarifa} \quad [\text{R\$/mês}]$$

2. **Custo com Tarifa Social ($C_{\text{social}}$):**
$$C_{\text{social}} = C_{\text{atual}} \times (1 - 0,65) \quad [\text{R\$/mês}]$$

**Exemplo prático (considerando consumo de $150,0\text{ kWh/mês}$):**
* $C_{\text{atual}} = 150,0\text{ kWh} \times R\$\ 0,85 = R\$\ 127,50/\text{mês}$
* $C_{\text{social}} = R\$\ 127,50 \times 0,35 = R\$\ 44,63/\text{mês}$ *(economia de $R\$\ 82,87/\text{mês}$)*

---

## 3. Comparação de Alternativas

Para atender às necessidades de bem-estar, saúde e suprimento energético no período de referência de **$1\text{ mês}$ ($30\text{ dias}$)**, o sistema compara o cenário base com duas alternativas estratégicas:

### Cenário Base: Tarifa Convencional e Custos Integrais
* **Descrição:** Residência atendida integralmente pela distribuidora local na tarifa cheia, sem acesso a subsídios sociais ou geração própria.
* **Custos e Consumo (base $150\text{ kWh/mês}$):** $150,0\text{ kWh/mês}$ | $R\$\ 127,50/\text{mês}$ (somados aos gastos particulares com saúde).
* **Impacto Ambiental:** Dependência total do Sistema Interligado Nacional (SIN), sujeito ao acionamento de usinas termelétricas fósseis em períodos de escassez hídrica e maior emissão indireta de $CO_2$.

### Alternativa 1: Tarifa Social de Energia Elétrica (TSEE) + Integração SUS
* **Descrição:** Enquadramento do idoso (inscrito no CadÚnico ou beneficiário do BPC/LOAS) na Tarifa Social de Energia Elétrica (desconto de até $65\%$), aliado à retirada gratuita de medicamentos essenciais (hipertensão e diabetes) pelo **Programa Farmácia Popular do SUS**.
* **Custos e Economia (base $150\text{ kWh/mês}$):**
  * Novo custo de energia: **$R\$\ 44,63/\text{mês}$**.
  * Economia direta na conta de luz: **$R\$\ 82,87/\text{mês}$** (além da economia de até $80\%$ em medicamentos contínuos).
* **Viabilidade:** Altíssima e imediata. Não exige investimento financeiro inicial ($\text{CAPEX} = R\$\ 0,00$), dependendo apenas da atualização cadastral junto ao CRAS e à concessionária.
* **Impacto Ambiental:** Incentiva o consumo consciente dentro das faixas de baixo consumo subsidiadas pela ANEEL.

### Alternativa 2: Fonte Renovável — Energia Solar Fotovoltaica Residencial
* **Descrição:** Instalação de sistema de microgeração solar fotovoltaica no telhado da residência do idoso (Geração Distribuída) para abater o consumo de aparelhos de climatização e cuidados médicos domiciliares.
* **Custos e Retorno:** Exige investimento inicial (ou linhas de financiamento específicas para aposentados), proporcionando redução de até **$80\%$ a $85\%$** na fatura mensal (pagando-se apenas a taxa mínima de disponibilidade da rede), com retorno financeiro (*payback*) estimado entre **$3,5\text{ e } 4,5\text{ anos}$**.
* **Viabilidade:** Média a Alta (a médio prazo). Depende de imóvel próprio com área de telhado sem sombreamento e avaliação técnica de engenharia.
* **Impacto Ambiental:** Geração de energia **$100\%$ limpa e renovável**, evitando emissões de gases de efeito estufa ao longo da vida útil dos painéis (superior a $25\text{ anos}$).

---

## 4. Integração ao Assistente (Citizen Assist AI)

Os cálculos e análises foram incorporados diretamente à arquitetura lógica do **Citizen Assist AI** por meio da estrutura modular em Python:

1. **Entrada Interativa (`assistente_virtual_saude`):** O sistema apresenta um menu simples no terminal e solicita ao usuário dados de fácil compreensão (consumo mensal em kWh ou gasto estimado com saúde), contando com tratamento de exceções (`try/except`) e validação contra valores negativos ou inválidos.
2. **Processamento Parametrizado (`calcular_impacto_saude_idoso`):** O valor informado alimenta automaticamente as equações de comparação entre a tarifa convencional, a Tarifa Social (desconto de $65\%$) e a geração solar fotovoltaica.
3. **Resposta Estruturada e Educativa:** O assistente retorna um relatório formatado contendo:
   * O diagnóstico do gasto mensal atual do idoso;
   * A simulação imediata de economia financeira via Tarifa Social e SUS;
   * A recomendação sustentável envolvendo energia solar residencial;
   * Os avisos transparentes de limitações e as fontes oficiais que validam os dados.

---

## 5. Referências e Limitações

### Referências Utilizadas
* **ANEEL (Agência Nacional de Energia Elétrica):** Diretrizes da Tarifa Social de Energia Elétrica (Lei nº 12.212/2010) e regras de microgeração distribuída (Lei nº 14.300/2022).
* **PROCEL / Eletrobras:** Tabela de Consumo Indicativo de Energia Elétrica de Equipamentos Residenciais.
* **Ministério da Saúde / Governo Federal:** Diretrizes do Programa Farmácia Popular do Brasil e Cadastro Único (CadÚnico / BPC).

### Condições e Limitações das Estimativas
Para garantir transparência técnica ao cidadão, o assistente explicita em todas as respostas que os valores calculados são **estimativas indicativas** e não resultados garantidos, estando sujeitos às seguintes condições:

* **Elegibilidade aos Programas Sociais:** A concessão do desconto de até $65\%$ na Tarifa Social exige inscrição ativa no CadÚnico (renda familiar per capita de até meio salário mínimo) ou recebimento do Benefício de Prestação Continuada (BPC) por idoso com 65 anos ou mais. O acesso à Farmácia Popular exige receita médica válida.
* **Variação Tarifária:** A tarifa média de $R\$\ 0,85/\text{kWh}$ utilizada no código varia conforme a distribuidora local, a região do país, impostos estaduais (ICMS) e bandeiras tarifárias vigentes.
* **Condições da Energia Solar:** A economia estimada com painéis fotovoltaicos depende do índice de irradiação solar da localidade, da orientação do telhado e do dimensionamento técnico do sistema.
* **Caráter Acadêmico:** Este assistente **não** possui vínculo oficial com o Governo Federal, Ministério da Saúde ou concessionárias de energia.
