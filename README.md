# Caching em Redes com Perdas e Atrasos

![GitHub repo size](https://img.shields.io/github/repo-size/siliconemonster/AD-TrabalhoFinal?color=a21360&style=for-the-badge)
![GitHub language count](https://img.shields.io/github/languages/count/siliconemonster/AD-TrabalhoFinal?color=a21360&style=for-the-badge)
![Made With](https://img.shields.io/badge/Made%20With-Python-lightgrey?color=a21360&style=for-the-badge)
![GitHub repo file count](https://img.shields.io/github/directory-file-count/siliconemonster/AD-TrabalhoFinal?color=a21360&style=for-the-badge)
![GitHub last commit](https://img.shields.io/github/last-commit/siliconemonster/AD-TrabalhoFinal?color=a21360&style=for-the-badge)

# Índice

- [Introdução](#introdução)
- [Microdados ENADE](#microdados-enade)
- [Obtenção dos Dados](#obtenção-dos-dados)
- [Modelagem de Dados](#modelagem-de-dados)
- [Tratamento de Dados](#tratamento-de-dados)
- [Análise Exploratória](#análise-exploratória)
- [Modelo de Predição](#modelo-de-predição)
- [Colaboradores](#colaboradores)
- [Licença](#licença)

# Introdução

O objetivo deste projeto é construir um simulador de um sistema de 2 caches em redes (FIFO, LRU, Random e Estáticas) com perdas e atrasos, de acordo com 4 cenários possíveis. São eles: canais sem perdas e sem atrasos, canais com perdas e sem atrasos, e dois canais com perdas e com atrasos.
Também devemos efetuar os cálculos utilizando nossos conhecimentos sobre cadeias de Markov, e compararmos com o intervalo de confiança obtido em cada simulação quando possível. Calcularemos as probabilidades de sucesso, user miss, tempo médio de serviço de requisições, conforme o que for solicitado em cada cenário. 

Optamos pela linguagem Python, pois fornece uma gama de funções pré-existentes para o nosso auxílio, entre elas funções para plotagem de gráficos.


# Colaboradores

<table>
  <tr>
    <td align="center">
      <a href="#">
        <img src="https://avatars.githubusercontent.com/u/20328857?v=4" width="100px;" alt="Foto de Aline Freie no GitHub"/><br>
        <sub>
          <a href="https://github.com/siliconemonster"><b>Aline Freire</b></a>
        </sub>
      </a>
    </td>
    <td align="center">
      <a href="#">
        <img src="https://avatars.githubusercontent.com/u/34246743?v=4" width="100px;" alt="Foto de Letícia Tavares no GitHub"/><br>
        <sub>
          <a href="https://github.com/leticiatavaresds"><b>Letícia Tavares</b></a>
        </sub>
      </a>
    </td>
</table>


[⬆ Voltar ao topo](#caching-em-redes-com-perdas-e-atrasos)

# Licença

The MIT License (MIT) 2022 - Aline Freire e Letícia Tavares. Leia o arquivo [LICENSE.md](https://github.com/siliconemonster/AD-TrabalhoFinal/blob/master/LICENSE.md) para mais detalhes.


[⬆ Voltar ao topo](#caching-em-redes-com-perdas-e-atrasos)
