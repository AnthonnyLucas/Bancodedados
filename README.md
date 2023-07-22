# Bancodedados criação das tabelas
CREATE TABLE Rotas (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    Nome VARCHAR(100) NOT NULL,
    Pontos TEXT NOT NULL
);

CREATE TABLE PosicoesUsuarios (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    Latitude DECIMAL(10, 8) NOT NULL,
    Longitude DECIMAL(11, 8) NOT NULL,
    Horario DATETIME NOT NULL,
    Rota_ID INT NOT NULL,
    FOREIGN KEY (Rota_ID) REFERENCES Rotas(ID)
);


import mysql.connector

# Função para inserir uma nova posição de usuário no banco de dados
def inserir_posicao_usuario(latitude, longitude, horario, rota_id):
    try:
        # Conecta ao banco de dados
        connection = mysql.connector.connect(
            host='SEU_HOST',
            user='SEU_USUARIO',
            password='SUA_SENHA',
            database='SEU_BANCO_DE_DADOS'
        )
        cursor = connection.cursor()

        # Insere a posição na tabela "PosicoesUsuarios"
        sql_query = "INSERT INTO PosicoesUsuarios (Latitude, Longitude, Horario, Rota_ID) VALUES (%s, %s, %s, %s)"
        data = (latitude, longitude, horario, rota_id)
        cursor.execute(sql_query, data)

        # Confirma a transação e fecha a conexão
        connection.commit()
        cursor.close()
        connection.close()

        print("Posição do usuário inserida com sucesso!")
    except mysql.connector.Error as error:
        print(f"Erro ao inserir posição do usuário: {error}")

# Exemplo de uso da função de inserção de posição do usuário
if __name__ == "__main__":
    # Valores de exemplo (latitude, longitude e horário do usuário)
    latitude_usuario = -22.9083
    longitude_usuario = -43.1964
    horario_usuario = "2023-07-21 12:34:56"  # Formato: "YYYY-MM-DD HH:MM:SS"
    rota_id_usuario = 1  # O ID da rota à qual o usuário pertence

    # Chama a função para inserir a posição do usuário no banco de dados
    inserir_posicao_usuario(latitude_usuario, longitude_usuario, horario_usuario, rota_id_usuario)
