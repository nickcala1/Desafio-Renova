import pandas as pd



# Carregar os dados
perfil_eleitorado = pd.read_csv('perfil_eleitorado_tratado.csv')
resultados = pd.read_csv('resultados_tratados.csv')

# Consulta 1: Em qual município o candidato X foi mais votado
def municipio_mais_votado_por_candidato(candidato_nome):
    candidato_municipio = resultados[resultados['Nome_Candidato'] == candidato_nome]
    municipio_mais_votado = candidato_municipio.loc[candidato_municipio['Votos'].idxmax()]
    return municipio_mais_votado

candidatoX_municipio = municipio_mais_votado_por_candidato('CandidatoX')

# Consulta 2: Candidato mais votado em cada município
max_votos_por_municipio = resultados.groupby(['ID_Municipio', 'Nome_Candidato'])['Votos'].max().reset_index()
candidato_mais_votado = max_votos_por_municipio.groupby('ID_Municipio').apply(lambda x: x[x['Votos'] == x['Votos'].max()])
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
plt.bar(candidato_mais_votado['ID_Municipio'], candidato_mais_votado['Votos'])
plt.xlabel('ID do Município')
plt.ylabel('Votos')
plt.title('Candidato Mais Votado em Cada Município')
plt.show()
# Consulta 3: Perfil do eleitorado que mais votou em cada candidato
perfil_eleitorado_votos = pd.merge(resultados, perfil_eleitorado, left_on='ID_Municipio', right_on='ID')
perfil_eleitorado_votos = perfil_eleitorado_votos.groupby(['Nome_Candidato', 'FaixaEtaria'])['Votos'].sum().reset_index()
