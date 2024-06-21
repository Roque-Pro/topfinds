# topfinds
Projto python automação para trazer top 10 palavras-chave mais comentadas da internet.


Descrição
Este projeto é um bot em Python que utiliza a API do Google Trends para identificar as 10 palavras-chave mais populares e as que estão em ascensão no Brasil sobre qualquer tema. Ele foi desenvolvido para fornecer insights valiosos sobre as tendências atuais, sendo especialmente útil para profissionais de marketing, analistas de dados e entusiastas de tecnologia.

Funcionalidades
Consulta de palavras-chave populares no Google Trends.
Identificação de palavras-chave em ascensão.
Foco específico nas tendências do Brasil.
Formatação clara e detalhada dos resultados.
Requisitos
Python 3.x
Biblioteca pytrends
Instalação
Clone o repositório:

bash
Copiar código
git clone https://github.com/seu-usuario/seu-repositorio.git
Navegue até o diretório do projeto:

bash
Copiar código
cd seu-repositorio
Instale a biblioteca pytrends:

bash
Copiar código
pip install pytrends
Uso
Abra o arquivo google_trends_analyzer.py e edite a variável keyword com o termo que você deseja pesquisar.

Execute o script:

bash
Copiar código
python google_trends_analyzer.py
O resultado será exibido no console, mostrando as 10 palavras-chave mais populares e as em ascensão no Brasil, junto com suas respectivas pontuações de popularidade e taxas de crescimento.

Exemplo de Código
python
Copiar código
from pytrends.request import TrendReq
import pandas as pd

def get_top_related_topics(keyword, num_results=10, geo='BR'):
    pytrends = TrendReq(hl='pt-BR', tz=360)
    pytrends.build_payload([keyword], cat=0, timeframe='now 1-d', geo=geo, gprop='')
    related_queries = pytrends.related_queries()
    
    if keyword in related_queries:
        top_related = related_queries[keyword]['top']
        rising_related = related_queries[keyword]['rising']
        
        if top_related is not None:
            top_related = top_related.head(num_results)
            rising_related = rising_related.head(num_results)
            top_related['value'] = top_related['value'].astype(str) + ' (pontuação de popularidade de 0 a 100)'
            rising_related['value'] = rising_related['value'].astype(str) + ' (crescimento percentual)'
            top_related.columns = ['Termo', 'Popularidade']
            rising_related.columns = ['Termo', 'Crescimento']
            detailed_related = pd.merge(top_related, rising_related, on='Termo', how='outer')
            return detailed_related
        else:
            return f"Nenhuma consulta relacionada encontrada para {keyword}"
    else:
        return f"Palavra-chave {keyword} não encontrada nas consultas relacionadas"

keyword = 'tecnologia'
top_topics = get_top_related_topics(keyword)
print(top_topics)
Contribuição
Contribuições são bem-vindas! Sinta-se à vontade para abrir issues e pull requests para sugerir melhorias ou correções.

Licença
Este projeto está licenciado sob a licença MIT. Consulte o arquivo LICENSE para obter mais informações.

