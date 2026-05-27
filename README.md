# 🛡️ Guardião de Habitats Extremos

Projeto desenvolvido para a **Global Solution de Physical Computing - IoT & IoB**.

## 🌌 Tema

**Guardião de Habitats Extremos - Lunar/Mars Colony**

O projeto simula um sistema de monitoramento atmosférico para habitats extremos, como colônias na Lua ou em Marte.

## 🎯 Objetivo

Classificar o ambiente interno de um habitat espacial em três estados:

- 🟢 **Seguro**
- 🟡 **Alerta**
- 🔴 **Crítico**

A classificação é feita a partir da pressão atmosférica simulada por um potenciômetro no Wokwi.

## 🚀 Problema

Em uma colônia lunar ou marciana, alterações na pressão atmosférica podem representar risco à vida dos astronautas. Por isso, é essencial detectar rapidamente situações de alerta ou emergência no ambiente interno do habitat.

## 💡 Solução

O projeto utiliza um **ESP32** com um **potenciômetro** no simulador **Wokwi**. O potenciômetro simula a pressão atmosférica do habitat, variando entre **50 kPa e 150 kPa**.

Os dados foram coletados e exportados em CSV para treinamento de um modelo no **Edge Impulse**. Após o treinamento, as regras aprendidas pelo modelo foram traduzidas para estruturas condicionais `if/else` no código C++.

O ESP32 também hospeda uma página HTML via **Webserver**, exibindo o status atualizado do ambiente.

## 📊 Engenharia de Dados

O dataset utilizado possui as seguintes colunas:

| Coluna | Descrição |
|---|---|
| `pressao_kpa` | Pressão atmosférica simulada |
| `desvio_pressao` | Diferença em relação à pressão ideal de 100 kPa |
| `label` | Classe do ambiente: Seguro, Alerta ou Critico |

Exemplo de dados utilizados:

| pressao_kpa | desvio_pressao | label |
|---|---|---|
| 100.00 | 0.00 | Seguro |
| 115.00 | 15.00 | Alerta |
| 140.00 | 40.00 | Critico |

## 🧠 TinyML

O modelo foi treinado no **Edge Impulse** para reconhecer os três estados do ambiente:

- Seguro
- Alerta
- Crítico

A biblioteca/modelo exportado do Edge Impulse está incluída no repositório como arquivo `.h`.

## ⚙️ Regras de Classificação

A pressão ideal considerada foi **100 kPa**.

| Estado | Regra |
|---|---|
| 🟢 Seguro | Desvio menor ou igual a 10 kPa |
| 🟡 Alerta | Desvio maior que 10 kPa e menor ou igual a 25 kPa |
| 🔴 Crítico | Desvio maior que 25 kPa |

Na prática:

| Estado | Faixa de Pressão |
|---|---|
| 🟢 Seguro | 90 a 110 kPa |
| 🟡 Alerta | 75 a 89 kPa ou 111 a 125 kPa |
| 🔴 Crítico | Abaixo de 75 kPa ou acima de 125 kPa |

Trecho da lógica implementada no ESP32:

    if (desvioAtual <= 10.0) {
      statusAtual = "Seguro";
    } 
    else if (desvioAtual <= 25.0) {
      statusAtual = "Alerta";
    } 
    else {
      statusAtual = "Critico";
    }

## 🌐 Webserver

O ESP32 cria uma página HTML que exibe:

- Pressão simulada;
- Desvio da pressão ideal;
- Status atual do habitat;
- Descrição da situação.

A página é atualizada automaticamente a cada **2 segundos**, permitindo que qualquer “astronauta” conectado à rede local acompanhe o estado do setor monitorado.

## 🧪 Simulação no Wokwi

Componentes utilizados:

- ESP32;
- Potenciômetro;
- Monitor Serial;
- Webserver ESP32.

### Ligações do circuito

| Potenciômetro | ESP32 |
|---|---|
| VCC | 3V3 |
| GND | GND |
| SIG | GPIO34 |

## 🌱 ODS

O projeto está alinhado ao **ODS 11 - Cidades e Comunidades Sustentáveis**, pois propõe uma solução de monitoramento para ambientes habitáveis, seguros e resilientes.

Embora o cenário seja espacial, a ideia também pode ser aplicada em ambientes extremos na Terra, como estações isoladas, laboratórios, áreas industriais e cidades inteligentes.

## 🛠️ Tecnologias Utilizadas

- ESP32
- Wokwi
- Edge Impulse
- TinyML
- Arduino C++
- HTML
- CSS
- Webserver ESP32
- GitHub

## 👨‍🚀 Integrantes

- Diana Letícia de Souza Inocencio -  RM553562
- João Viktor Carvalho de Souza - RM552613
- Victor Augusto Pereira dos Santos -  RM553518

## 🔗 Links

- 🌐 Projeto Wokwi:  https://wokwi.com/projects/465120609913711617

- 🎥 Vídeo demonstrativo: https://youtu.be/uDPULQC-IUI


## 🛰️ Conclusão

O **Guardião de Habitats Extremos** demonstra como sensores, inteligência artificial na borda e sistemas embarcados podem ser utilizados para aumentar a segurança em ambientes críticos.

A solução permite identificar rapidamente anomalias atmosféricas e informar o estado do habitat em tempo real, contribuindo para a proteção dos astronautas e para a sustentabilidade de futuras colônias espaciais.
