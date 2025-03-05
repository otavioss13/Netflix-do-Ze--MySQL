Otávio Santos de Souza

#Código em My SQL - Netflix do Zé

CREATE DATABASE netflix_do_ze_otavio;

use netflix_do_ze_otavio;

CREATE TABLE tb_planos (
    id_plano int PRIMARY KEY AUTO_INCREMENT,
    descricao_plano varchar(30),
    valor decimal(11,2)
);

CREATE TABLE tb_generos (
    id_genero int PRIMARY KEY AUTO_INCREMENT,
    descricao_genero varchar(20)
);

CREATE TABLE tb_usuarios (
    id_usuario int PRIMARY KEY AUTO_INCREMENT,
    nome varchar(50),
    login varchar(30),
    email varchar(50),
    data_nascimento date,
    plano_id int,
    genero_id int,
    UNIQUE (login, email)
);

CREATE TABLE tb_midias (
    id_midia int PRIMARY KEY AUTO_INCREMENT,
    titulo_midia varchar(30),
    descricao_midia varchar(200),
    tipo varchar(20) CHECK (tipo IN ('Filme', 'Serie', 'Documentario', 'Desenho')),
    valor decimal(11,2),
    nota float,
    duracao int,
    faixa_etaria int,
    numero_temporadas int
);

CREATE TABLE tb_temporadas (
    id_temporada int PRIMARY KEY AUTO_INCREMENT,
    midia_id int,
    descricao_temporada varchar(200),
    numero_temporada int
);

CREATE TABLE tb_episodios (
    id_episodio int PRIMARY KEY AUTO_INCREMENT,
    temporada_id int,
    titulo_episodio varchar(30),
    descricao_episodio varchar(50),
    numero_episodio int,
    duracao int
);

CREATE TABLE tb_caracteristicas (
    id_caracteristica int PRIMARY KEY AUTO_INCREMENT,
    descricao varchar(30)
);

CREATE TABLE tb_midia_caract (
    id_midia_caract int PRIMARY KEY AUTO_INCREMENT,
    midia_id int,
    caracteristica_id int
);


#===============================================
# Criação das constraints (chaves estrangeiras)
#===============================================

ALTER TABLE tb_usuarios ADD CONSTRAINT FK_usuario_plano
    FOREIGN KEY (plano_id)
    REFERENCES tb_planos (id_plano);
 
ALTER TABLE tb_usuarios ADD CONSTRAINT FK_usuario_genero
    FOREIGN KEY (genero_id)
    REFERENCES tb_generos (id_genero);
 
ALTER TABLE tb_episodios ADD CONSTRAINT FK_epeisodio_temporada
    FOREIGN KEY (temporada_id)
    REFERENCES tb_temporadas (id_temporada);
 
ALTER TABLE tb_midia_caract ADD CONSTRAINT FK_midia_caract
    FOREIGN KEY (midia_id)
    REFERENCES tb_midias (id_midia);
 
ALTER TABLE tb_midia_caract ADD CONSTRAINT FK_caract_midia
    FOREIGN KEY (caracteristica_id)
    REFERENCES tb_caracteristicas (id_caracteristica);
 
ALTER TABLE tb_temporadas ADD CONSTRAINT FK_temporada_midia
    FOREIGN KEY (midia_id)
    REFERENCES tb_midias (id_midia);

#===============================
# Alguns inserts
#===============================

INSERT INTO tb_planos (descricao_plano, valor)
VALUES ('Gratuito', 0), ('Básico', 19.90), ('Padrão', 27.90), ('Premium', 37.90), ('Ultra', 45.90);

INSERT INTO tb_generos (descricao_genero)
VALUES ('Feminino'), ('Masculino'), ('Outro'), ('Prefiro não opinar');

INSERT INTO tb_usuarios (nome, login, email, data_nascimento, plano_id, genero_id) VALUES
('João Silva', 'joaosilva', 'joao.silva@email.com', '1990-01-15', 1, 2),
('Maria Souza', 'mariasouza', 'maria.souza@email.com', '1985-07-22', 2, 1),
('Carlos Oliveira', 'carlosoliveira', 'carlos.oliveira@email.com', '1992-03-11', 1, 2),
('Ana Pereira', 'anapereira', 'ana.pereira@email.com', '1988-09-30', 3, 1),
('Paulo Santos', 'paulosantos', 'paulo.santos@email.com', '1978-05-05', 1, 2),
('Fernanda Lima', 'fernandalima', 'fernanda.lima@email.com', '1995-12-17', 2, 1),
('Ricardo Mota', 'ricardomota', 'ricardo.mota@email.com', '1983-11-25', 3, 3),
('Beatriz Almeida', 'beatrizalmeida', 'beatriz.almeida@email.com', '1990-08-08', 1, 4),
('Darci Silva', 'darcisilva', 'darci.silva@gmail.com', '2000-07-22', 5, 3);

INSERT INTO tb_midias (titulo_midia, descricao_midia, tipo, valor, nota, duracao, faixa_etaria, numero_temporadas) VALUES
('Guerra Espacial', 'Uma série sobre viagens pelo universo.', 'Serie', 0, 8.5, 120, 12, 3),
('Mistério na Floresta', 'Documentário sobre a biodiversidade das florestas.', 'Documentario', 15.50, 7.8, 90, 10, 0),
('Heróis da Galáxia', 'Um grupo de heróis enfrenta desafios intergalácticos.', 'Filme', 0, 9.0, 180, 14, 0),
('Culinária para Todos', 'Programa de culinária para iniciantes e profissionais.', 'Serie', 0, 8.2, 45, 0, 4),
('Aventuras no Deserto', 'Filme sobre exploradores enfrentando desafios no deserto.', 'Filme', 0, 7.5, 130, 12, 0),
('Caminho para a Vitória', 'Drama sobre a jornada de um atleta olímpico.', 'Filme', 0, 8.2, 115, 10, 0),
('Noite de Terror', 'Filme de terror em uma casa assombrada.', 'Filme', 10.00, 6.9, 100, 16, 0),
('Segredos do Oceano', 'Documentário sobre os mistérios dos oceanos.', 'Documentario', 0, 8.0, 50, 10, 0),
('Cidade das Sombras', 'Série sobre detetives investigando crimes em uma cidade misteriosa.', 'Serie', 0, 9.1, 45, 16, 2),
('Detetives Urbanos', 'Filme policial que acompanha detetives resolvendo crimes na cidade.', 'Filme', 0, 8.0, 45, 14, 0),
('Vida Selvagem', 'Documentário sobre a fauna e flora de diferentes regiões do mundo.', 'Documentario', 0, 7.9, 50, 0, 0),
('Saga dos Reinos', 'Série épica sobre a luta de reinos rivais por poder.', 'Serie', 0, 9.3, 50, 14, 3);

INSERT INTO tb_temporadas (midia_id, descricao_temporada, numero_temporada) VALUES
(1, 'Primeira temporada da série Guerra Espacial', 1),
(1, 'Segunda temporada da série Guerra Espacial', 2),
(1, 'Terceira temporada da série Guerra Espacial', 3),
(4, 'Primeira temporada de Culinária para Todos', 1),
(4, 'Segunda temporada de Culinária para Todos', 2),
(4, 'Terceira temporada de Culinária para Todos', 3),
(4, 'Quarta temporada de Culinária para Todos', 4),
(9, 'Primeira temporada da série Cidade das Sombras', 1),
(9, 'Segunda temporada da série Cidade das Sombras', 2),
(12, 'Primeira temporada da série Saga dos Reinos', 1),
(12, 'Segunda temporada da série Saga dos Reinos', 2),
(12, 'Terceira temporada da série Saga dos Reinos', 3);


INSERT INTO tb_episodios (temporada_id, titulo_episodio, descricao_episodio, numero_episodio, duracao) VALUES
(1, 'O Início da Jornada', 'Primeiro episódio da série', 1, 60),
(1, 'Descobertas Intergalácticas', 'A equipe encontra uma nova galáxia', 2, 55),
(2, 'Novos planetas', 'Episódio inicial da segunda temporada', 1, 50),
(2, 'A Volta para Casa', 'Episódio final da segunda temporada', 2, 50),
(3, 'Explorando o Desconhecido', 'Primeiro episódio da terceira temporada', 1, 65),
(3, 'O fim dos tempos', 'Último episódio da terceira temporada', 2, 65),
(4, 'Preparando uma Receita Básica', 'Introdução à culinária', 1, 30),
(4, 'Receitas Intermediárias', 'Aprimorando técnicas', 2, 35),
(4, 'Receitas Avançadas', 'Seja um chef', 3, 35),
(5, 'Receitas Doces 1', 'Aprendendo sobremesas', 1, 30),
(5, 'Receitas Doces 2', 'Aprendendo sobremesas', 2, 30),
(5, 'Receitas Doces 3', 'Aprendendo sobremesas', 3, 30),
(6, 'Receitas Salgadas 1', 'Aprendendo pratos avançados', 1, 30),
(6, 'Receitas Salgadas 2', 'Aprendendo pratos avançados', 2, 30),
(6, 'Receitas Salgadas 3', 'Aprendendo pratos avançados', 3, 30),
(7, 'Master Chef 1', 'Desafio das receitas', 1, 30),
(7, 'Master Chef 2', 'Desafio das receitas', 2, 30),
(7, 'Master Chef 3', 'Desafio das receitas', 3, 30),
(8, 'Cidade das Sombras 1', 'Primeiro episódio da temporada', 1, 30),
(8, 'Cidade das Sombras 1', 'Segundo episódio da temporada', 2, 30),
(8, 'Cidade das Sombras 1', 'Terceiro episódio da temporada', 3, 30),
(9, 'Cidade das Sombras 2', 'Primeiro episódio da temporada', 1, 30),
(9, 'Cidade das Sombras 2', 'Segundo episódio da temporada', 2, 30),
(9, 'Cidade das Sombras 2', 'Terceiro episódio da temporada', 3, 30),
(10, 'O Começo do Conflito', 'Primeiro episódio da temporada', 1, 50),
(10, 'A Ascensão do Herói', 'Personagem principal começa a se destacar', 2, 48),
(10, 'Inimigos em Ascensão', 'Os reinos se preparam para a guerra', 3, 52),
(10, 'A Primeira Batalha', 'Primeiro confronto entre os reinos', 4, 55),
(11, 'Retorno dos Guerreiros', 'Os guerreiros se preparam para novos desafios', 1, 50),
(11, 'Segredos Revelados', 'Mistérios são descobertos', 2, 49),
(11, 'Batalha pelo Trono', 'Os reinos enfrentam uma luta intensa', 3, 53),
(11, 'A Conquista Final', 'A batalha épica entre os reinos', 4, 54),
(12, 'Nova Ordem', 'Um novo líder emerge', 1, 51),
(12, 'Antigas Alianças', 'Os reinos revisitam suas alianças', 2, 49),
(12, 'O Último Confronto', 'Os reinos entram em confronto novamente', 3, 52),
(12, 'A Paz Final', 'Conclusão da saga e busca pela paz', 4, 57);

INSERT INTO tb_caracteristicas (descricao) VALUES
('Ação'), ('Aventura'), ('Drama'), ('Comédia'), ('Terror'), ('Suspense'), ('Romance'), ('Ficção Científica'), ('Fantasia'), ('Documentário'), ('Mistério'), ('Histórico'), ('Musical'), ('Guerra'), ('Animação'), ('Biografia'), ('Esporte'), ('Família'), ('Policial'), ('Western'), ('Crime'), ('Thriller Psicológico'), ('Realidade Virtual'), ('Distopia'), ('Super-heróis'), ('Culinária'), ('Viagem no Tempo'), ('Medieval'), ('Espionagem'), ('Apocalíptico');

INSERT INTO tb_midia_caract (midia_id, caracteristica_id) VALUES
(1, 8), (1, 14), (1, 27), 
(2, 3), (2, 6), (2, 10), 
(3, 1), (3, 2), (3, 8), 
(4, 4), (4, 18), (4, 26), 
(5, 2), (5, 11), (5, 28), 
(6, 3), (6, 16), (6, 17), 
(7, 5), (7, 6), (7, 11), 
(8, 10), (8, 11), (8, 12), 
(9, 6), (9, 19), (9, 21), 
(10, 19), (10, 21), (10, 29), 
(11, 10), (11, 12), (11, 20), 
(12, 9), (12, 27), (12, 28);

CREATE TABLE tb_historico (
id_historico int PRIMARY KEY AUTO_INCREMENT,
duracao int,
usuario_id int,
midia_id int,
constraint Fk_historico_midia FOREIGN KEY (midia_id) REFERENCES tb_midias (id_midia),
constraint Fk_historico_usuario FOREIGN KEY (usuario_id) REFERENCES tb_usuarios (id_usuario)
);

ALTER TABLE tb_episodios
modify column descricao_episodio VARCHAR (100);

INSERT INTO tb_usuarios (nome, login, email, data_nascimento, plano_id, genero_id) VALUES
('Lucas Costa', 'lucascosta', 'lucas.costa@email.com', '1993-04-19', 2, 2),
('Juliana Rocha', 'julianarocha', 'juliana.rocha@email.com', '1990-02-27', 1, 1),
('Ricardo Silva', 'ricardosilva', 'ricardo.silva@email.com', '1987-11-05', 3, 2),
('Larissa Gomes', 'larissagomes', 'larissa.gomes@email.com', '1997-09-16', 4, 1),
('Fernando Alves', 'fernandoalves', 'fernando.alves@email.com', '1982-03-08', 2, 2),
('Sabrina Martins', 'sabrinamartins', 'sabrina.martins@email.com', '1994-10-12', 5, 1),
('Eduardo Pereira', 'eduardopereira', 'eduardo.pereira@email.com', '1989-05-03', 1, 3),
('Isabela Oliveira', 'isabelaoliveira', 'isabela.oliveira@email.com', '1996-11-22', 3, 1),
('Gabriel Souza', 'gabrielsouza', 'gabriel.souza@email.com', '1992-06-15', 2, 4),
('Tatiane Lima', 'tatianelima', 'tatiane.lima@email.com', '1984-01-29', 4, 2),
('Otavio Souza', 'otaviosouza', 'otaviosouza@email.com', '1995-08-30', 1, 2);

INSERT INTO tb_midias (titulo_midia, descricao_midia, tipo, valor, nota, duracao, faixa_etaria, numero_temporadas) VALUES
('Mundos Perdidos', 'Uma série sobre descobertas arqueológicas em locais perdidos.', 'Serie', 0, 8.6, 55, 12, 2),
('Expedição Polar', 'Documentário sobre expedições ao Polo Norte e Polo Sul.', 'Documentario', 12.00, 7.4, 60, 10, 0),
('O Último Imperador', 'Filme histórico sobre a queda de um império antigo.', 'Filme', 20.00, 9.2, 130, 14, 0),
('Aventuras Subaquáticas', 'Filme sobre exploração submarina e mistérios do fundo do mar.', 'Filme', 15.00, 8.5, 140, 10, 0),
('Tecnologia do Futuro', 'Documentário sobre inovações tecnológicas.', 'Documentario', 0, 7.8, 50, 16, 0),
('Estrada para a Liberdade', 'Filme sobre a história de uma revolução política.', 'Filme', 0, 8.3, 110, 12, 0),
('Guerra de Clãs', 'Série épica sobre famílias lutando pelo poder em um reino medieval.', 'Serie', 0, 9.1, 50, 14, 3),
('Mistérios da Mente', 'Documentário sobre psicologia e mistérios da mente humana.', 'Documentario', 0, 8.0, 45, 12, 0),
('Viagem no Tempo', 'Série de ficção científica sobre viagens no tempo.', 'Serie', 0, 8.7, 60, 16, 2),
('A Terra Prometida', 'Filme sobre a fundação de uma nova civilização.', 'Filme', 25.00, 9.0, 135, 16, 0);

INSERT INTO tb_temporadas (midia_id, descricao_temporada, numero_temporada) VALUES
(13, 'Primeira temporada de Mundos Perdidos', 1),
(13, 'Segunda temporada de Mundos Perdidos', 2),
(14, 'Primeira temporada de Expedição Polar', 1),
(15, 'Primeira temporada de O Último Imperador', 1),
(16, 'Primeira temporada de Aventuras Subaquáticas', 1),
(17, 'Primeira temporada de Tecnologia do Futuro', 1),
(18, 'Primeira temporada de Estrada para a Liberdade', 1),
(19, 'Primeira temporada de Guerra de Clãs', 1),
(19, 'Segunda temporada de Guerra de Clãs', 2),
(20, 'Primeira temporada de Mistérios da Mente', 1),
(20, 'Segunda temporada de Mistérios da Mente', 2),
(21, 'Primeira temporada de Viagem no Tempo', 1),
(21, 'Segunda temporada de Viagem no Tempo', 2),
(22, 'Primeira temporada de A Terra Prometida', 1);

INSERT INTO tb_episodios (temporada_id, titulo_episodio, descricao_episodio, numero_episodio, duracao) VALUES
(13, 'Os Mistérios dos Mausoléus', 'Os arqueólogos começam a investigar o primeiro local perdido', 1, 50),
(13, 'A Cidade Submersa', 'Exploração de uma cidade submersa nas profundezas da selva', 2, 55),
(14, 'O Perigo no Ártico', 'Os cientistas enfrentam uma tempestade no Ártico', 1, 60),
(15, 'A Queda do Império', 'A ascensão e queda de uma civilização antiga', 1, 60),
(16, 'A Expedição Profunda', 'Exploradores enfrentam perigos no fundo do oceano', 1, 70),
(17, 'Inovações em Robótica', 'Documentário sobre os avanços mais recentes na robótica', 1, 40),
(18, 'A Revolução do Século', 'Os eventos históricos que mudaram o rumo de um país', 1, 50),
(19, 'O Desafio dos Clãs', 'Primeiro grande confronto entre as famílias', 1, 60),
(19, 'Segredos do Castelo', 'Mistérios e intrigas no castelo do líder de um clã', 2, 55),
(20, 'O Cérebro Humano', 'Exploração do funcionamento do cérebro humano', 1, 45),
(20, 'Distúrbios Mentais', 'A análise dos distúrbios psicológicos mais comuns', 2, 50),
(21, 'Desvendando o Tempo', 'Cientistas começam a investigar o conceito de viagens no tempo', 1, 60),
(21, 'O Passado e o Futuro', 'Dúvidas surgem sobre as viagens temporais', 2, 58),
(22, 'Fundação e Desafios', 'A criação da nova sociedade e os desafios enfrentados', 1, 65);

INSERT INTO tb_midia_caract (midia_id, caracteristica_id) VALUES

(13, 8),  -- Ficção Científica
(13, 14), -- Aventura
(13, 27), -- Viagem no Tempo

-- Expedição Polar (Documentário)
(14, 10), -- Documentário
(14, 20), -- Natureza
(14, 11), -- Mistério

-- O Último Imperador (Filme)
(15, 1),  -- Ação
(15, 2),  -- Aventura
(15, 12), -- Histórico

-- Aventuras Subaquáticas (Filme)
(16, 4),  -- Comédia
(16, 28), -- Apocalíptico
(16, 5),  -- Aventura

-- Tecnologia do Futuro (Documentário)
(17, 10), -- Documentário
(17, 16), -- Futurismo
(17, 7),  -- Ciência

-- Estrada para a Liberdade (Filme)
(18, 1),  -- Ação
(18, 2),  -- Aventura
(18, 12), -- Histórico

-- Guerra de Clãs (Série)
(19, 9),  -- Guerra
(19, 18), -- Medieval
(19, 27), -- Drama

-- Mistérios da Mente (Documentário)
(20, 10), -- Documentário
(20, 6),  -- Suspense
(20, 22), -- Psicologia

-- Viagem no Tempo (Série)
(21, 8),  -- Ficção Científica
(21, 27), -- Viagem no Tempo
(21, 14), -- Aventura

-- A Terra Prometida (Filme)
(22, 1),  -- Ação
(22, 2),  -- Aventura
(22, 28), -- História
(22, 5);  -- Drama

INSERT INTO tb_historico (duracao, usuario_id, midia_id) VALUES
(120, 1, 1),   -- João Silva assistiu a "Guerra Espacial" por 120 minutos
(90, 2, 2),    -- Maria Souza assistiu a "Mistério na Floresta" por 90 minutos
(180, 3, 3),   -- Carlos Oliveira assistiu a "Heróis da Galáxia" por 180 minutos
(45, 4, 4),    -- Ana Pereira assistiu a "Culinária para Todos" por 45 minutos
(130, 5, 5),   -- Paulo Santos assistiu a "Aventuras no Deserto" por 130 minutos
(115, 6, 6),   -- Fernanda Lima assistiu a "Caminho para a Vitória" por 115 minutos
(100, 7, 7),   -- Ricardo Mota assistiu a "Noite de Terror" por 100 minutos
(50, 8, 8),    -- Beatriz Almeida assistiu a "Segredos do Oceano" por 50 minutos
(45, 9, 9),    -- Darci Silva assistiu a "Cidade das Sombras" por 45 minutos
(45, 10, 10),  -- Lucas Costa assistiu a "Detetives Urbanos" por 45 minutos
(50, 11, 11),  -- Juliana Rocha assistiu a "Vida Selvagem" por 50 minutos
(50, 12, 12),  -- Ricardo Silva assistiu a "Saga dos Reinos" por 50 minutos
(55, 13, 13),  -- Larissa Gomes assistiu a "Mundos Perdidos" por 55 minutos
(60, 14, 14),  -- Fernando Alves assistiu a "Expedição Polar" por 60 minutos
(130, 15, 15), -- Sabrina Martins assistiu a "O Último Imperador" por 130 minutos
(140, 16, 16), -- Eduardo Pereira assistiu a "Aventuras Subaquáticas" por 140 minutos
(50, 17, 17),  -- Isabela Oliveira assistiu a "Tecnologia do Futuro" por 50 minutos
(110, 18, 18), -- Gabriel Souza assistiu a "Estrada para a Liberdade" por 110 minutos
(50, 19, 19),  -- Tatiane Lima assistiu a "Guerra de Clãs" por 50 minutos
(90, 1, 3),    -- João Silva assistiu a "Heróis da Galáxia" por 90 minutos
(120, 2, 4),   -- Maria Souza assistiu a "Culinária para Todos" por 120 minutos
(150, 3, 5),   -- Carlos Oliveira assistiu a "Aventuras no Deserto" por 150 minutos
(75, 4, 6),    -- Ana Pereira assistiu a "Caminho para a Vitória" por 75 minutos
(100, 5, 7),   -- Paulo Santos assistiu a "Noite de Terror" por 100 minutos
(85, 6, 8),    -- Fernanda Lima assistiu a "Segredos do Oceano" por 85 minutos
(60, 7, 9),    -- Ricardo Mota assistiu a "Cidade das Sombras" por 60 minutos
(50, 8, 10),   -- Beatriz Almeida assistiu a "Detetives Urbanos" por 50 minutos
(120, 9, 11),  -- Darci Silva assistiu a "Vida Selvagem" por 120 minutos
(110, 10, 12), -- Lucas Costa assistiu a "Saga dos Reinos" por 110 minutos
(100, 1, 10),  -- João Silva assistiu a "Detetives Urbanos" por 100 minutos
(130, 2, 12),  -- Maria Souza assistiu a "Saga dos Reinos" por 130 minutos
(90, 3, 9),    -- Carlos Oliveira assistiu a "Cidade das Sombras" por 90 minutos
(115, 4, 11),  -- Ana Pereira assistiu a "Vida Selvagem" por 115 minutos
(80, 5, 14),   -- Paulo Santos assistiu a "Expedição Polar" por 80 minutos
(140, 6, 13),  -- Fernanda Lima assistiu a "Mundos Perdidos" por 140 minutos
(110, 7, 15),  -- Ricardo Mota assistiu a "O Último Imperador" por 110 minutos
(60, 8, 17),   -- Beatriz Almeida assistiu a "Tecnologia do Futuro" por 60 minutos
(50, 9, 16),   -- Darci Silva assistiu a "Aventuras Subaquáticas" por 50 minutos
(130, 10, 18), -- Lucas Costa assistiu a "Estrada para a Liberdade" por 130 minutos
(100, 20, 18), -- Otavio Souza assistiu a "Estrada para a Liberdade" por 130 minutos
(45, 20, 16);   -- Otavio Souza assistiu a "Aventuras Subaquáticas" por 50 minutos

CREATE TABLE tb_avaliacao (
nota float,
midia_id int,
usuario_id int,
caracteristica_id int,
constraint Fk_avaliacao_midia FOREIGN KEY (midia_id) REFERENCES tb_midias (id_midia),
constraint Fk_avaliacao_usuario FOREIGN KEY (usuario_id) REFERENCES tb_usuarios (id_usuario),
constraint Fk_avaliacao_caract FOREIGN KEY (caracteristica_id) REFERENCES tb_caracteristicas (id_caracteristica)
);

INSERT INTO tb_avaliacao (nota, midia_id, usuario_id, caracteristica_id) VALUES
(9.5, 3, 3, 8),  -- Heróis da Galáxia -> Ficção Científica
(9.4, 10, 9, 19), -- Detetives Urbanos -> Policial
(9.3, 9, 9, 21),  -- Cidade das Sombras -> Crime
(9.2, 19, 19, 18), -- Guerra de Clãs -> Família
(9.2, 5, 3, 12),  -- Aventuras no Deserto -> Histórico
(9.2, 19, 18, 27), -- Guerra de Clãs -> Viagem no Tempo
(9.1, 13, 11, 14), -- Mundos Perdidos -> Guerra
(9.1, 17, 16, 16), -- Tecnologia do Futuro -> Biografia
(9.0, 15, 15, 17), -- O Último Imperador -> Esporte
(8.9, 12, 12, 9),  -- Saga dos Reinos -> Fantasia
(8.8, 18, 18, 12), -- Estrada para a Liberdade -> Histórico
(8.8, 15, 14, 5),  -- O Último Imperador -> Terror
(8.7, 16, 16, 28), -- Aventuras Subaquáticas -> Apocalíptico
(8.7, 9, 7, 6),   -- Cidade das Sombras -> Suspense
(8.7, 13, 12, 27), -- Mundos Perdidos -> Viagem no Tempo
(8.6, 11, 10, 12), -- Vida Selvagem -> Histórico
(8.5, 6, 6, 16),  -- Caminho para a Vitória -> Biografia
(8.5, 6, 4, 6),   -- Caminho para a Vitória -> Suspense
(8.5, 4, 3, 4),   -- Culinária para Todos -> Comédia
(8.3, 13, 13, 8), -- Mundos Perdidos -> Ficção Científica
(8.3, 7, 5, 2),   -- Noite de Terror -> Aventura
(8.3, 18, 17, 3), -- Estrada para a Liberdade -> Drama
(8.2, 2, 2, 3),   -- Mistério na Floresta -> Drama
(8.2, 14, 12, 10), -- Expedição Polar -> Documentário
(8.2, 7, 6, 17),  -- Noite de Terror -> Esporte
(8.1, 10, 10, 20), -- Detetives Urbanos -> Western
(8.1, 3, 1, 7),   -- Heróis da Galáxia -> Romance
(8.1, 2, 1, 1),   -- Mistério na Floresta -> Ação
(8.0, 8, 8, 1),   -- Segredos do Oceano -> Ação
(8.0, 11, 9, 9),  -- Vida Selvagem -> Fantasia
(8.0, 9, 8, 12),  -- Cidade das Sombras -> Histórico
(8.0, 3, 19, 29), -- Heróis da Galáxia -> Apocalíptico
(7.9, 8, 6, 6),   -- Segredos do Oceano -> Suspense
(7.9, 3, 2, 2),   -- Heróis da Galáxia -> Aventura
(7.9, 3, 20, 4),  -- Heróis da Galáxia -> Comédia
(7.8, 5, 5, 4),   -- Aventuras no Deserto -> Comédia
(7.8, 12, 10, 16), -- Saga dos Reinos -> Biografia
(7.8, 5, 4, 7),   -- Aventuras no Deserto -> Romance
(7.6, 4, 2, 5),   -- Culinária para Todos -> Terror
(7.5, 1, 1, 8),   -- Guerra Espacial -> Ficção Científica
(7.5, 11, 11, 10), -- Vida Selvagem -> Documentário
(7.5, 20, 20, 22), -- A Terra Prometida -> Thriller Psicológico
(7.5, 16, 15, 5),  -- Aventuras Subaquáticas -> Terror
(7.4, 17, 17, 10), -- Tecnologia do Futuro -> Documentário
(7.4, 8, 7, 11),  -- Noite de Terror -> Mistério
(7.3, 15, 13, 1),  -- O Último Imperador -> Ação
(7.3, 3, 11, 28), -- Heróis da Galáxia -> Apocalíptico
(7.2, 7, 7, 6),   -- Noite de Terror -> Suspense
(7.2, 10, 8, 18), -- Estrada para a Liberdade -> Família
(7.2, 14, 13, 7), -- Expedição Polar -> Romance
(7.0, 4, 4, 16), -- Culinária para Todos -> Biografia
(7.0, 14, 14, 6), -- Expedição Polar -> Suspense
(6.9, 3, 5, 12);  -- Heróis da Galáxia -> Histórico

CREATE TABLE tb_recomendados (
id_recomendados INT PRIMARY KEY AUTO_INCREMENT ,
tipo_recomendacao VARCHAR (50),
data_recomendacao DATETIME,
nota_media_recomendada float ,
usuario_id int, 
constraint Fk_recomendado_usuario FOREIGN KEY (usuario_id) REFERENCES tb_usuarios (id_usuario)
);



CREATE TABLE tb_recomendados_midia (
midia_id int,
recomendados_id int,
caracteristicas_id int,
constraint Fk_midia_recomendados FOREIGN KEY (midia_id) REFERENCES tb_midias (id_midia),
constraint Fk_recomendados_recomendados_midia FOREIGN KEY (recomendados_id) REFERENCES tb_recomendados (id_recomendados) ,
constraint Fk_caracteristicas_recomendados FOREIGN KEY (caracteristicas_id) REFERENCES tb_caracteristicas (id_caracteristica)
);

INSERT INTO tb_recomendados (tipo_recomendacao, nota_media_recomendada, usuario_id, data_recomendacao)
VALUES 
('semelhante', 8.5, 1, '2024-11-01 10:00:00'),   -- João Silva
('semelhante', 7.8, 2, '2024-11-01 10:05:00'),   -- Maria Souza
('semelhante', 9.0, 3, '2024-11-01 10:10:00'),   -- Carlos Oliveira
('semelhante', 7.4, 4, '2024-11-01 10:15:00'),   -- Ana Pereira
('semelhante', 8.2, 5, '2024-11-01 10:20:00'),   -- Paulo Santos
('popular', 8.9, 6, '2024-11-02 11:00:00'),      -- Fernanda Lima
('popular', 7.2, 7, '2024-11-02 11:05:00'),      -- Ricardo Mota
('popular', 9.3, 8, '2024-11-02 11:10:00'),      -- Beatriz Almeida
('popular', 8.1, 9, '2024-11-02 11:15:00'),      -- Darci Silva
('popular', 7.5, 10, '2024-11-02 11:20:00'),     -- Lucas Costa
('novidade', 9.3, 11, '2024-11-03 12:00:00'),    -- Juliana Rocha
('novidade', 8.9, 12, '2024-11-03 12:05:00'),    -- Ricardo Silva
('novidade', 8.5, 13, '2024-11-03 12:10:00'),    -- Larissa Gomes
('novidade', 8.8, 14, '2024-11-03 12:15:00'),    -- Fernando Alves
('novidade', 9.0, 15, '2024-11-03 12:20:00'),    -- Sabrina Martins
('novidade', 8.7, 1, '2024-11-04 13:00:00'),     -- João Silva
('novidade', 8.0, 2, '2024-11-04 13:05:00'),     -- Maria Souza
('novidade', 9.1, 3, '2024-11-04 13:10:00'),     -- Carlos Oliveira
('novidade', 7.8, 4, '2024-11-04 13:15:00'),     -- Ana Pereira
('novidade', 8.2, 5, '2024-11-04 13:20:00'),     -- Paulo Santos
('nota_alta', 9.2, 1, '2024-11-05 14:00:00'),    -- João Silva
('nota_alta', 8.7, 2, '2024-11-05 14:05:00'),    -- Maria Souza
('nota_alta', 9.4, 3, '2024-11-05 14:10:00'),    -- Carlos Oliveira
('nota_alta', 8.5, 4, '2024-11-05 14:15:00'),    -- Ana Pereira
('nota_alta', 9.0, 5, '2024-11-05 14:20:00'),    -- Paulo Santos
('gostos_semelhantes', 8.3, 6, '2024-11-06 15:00:00'),   -- Fernanda Lima
('gostos_semelhantes', 8.0, 7, '2024-11-06 15:05:00'),   -- Ricardo Mota
('gostos_semelhantes', 8.9, 8, '2024-11-06 15:10:00'),   -- Beatriz Almeida
('gostos_semelhantes', 7.8, 9, '2024-11-06 15:15:00'),   -- Darci Silva
('gostos_semelhantes', 9.0, 10, '2024-11-06 15:20:00'),  -- Lucas Costa
('filmes_populares', 8.6, 11, '2024-11-07 16:00:00'),    -- Juliana Rocha
('filmes_populares', 8.1, 12, '2024-11-07 16:05:00'),    -- Ricardo Silva
('filmes_populares', 9.2, 13, '2024-11-07 16:10:00'),    -- Larissa Gomes
('filmes_populares', 8.4, 14, '2024-11-07 16:15:00'),    -- Fernando Alves
('filmes_populares', 9.3, 15, '2024-11-07 16:20:00');    -- Sabrina Martins

INSERT INTO tb_recomendados_midia (midia_id, recomendados_id, caracteristicas_id) VALUES
(1, 1, 2),  -- "Guerra Espacial" recomendado para João Silva com a característica 2
(2, 2, 3),  -- "Mistério na Floresta" recomendado para Maria Souza com a característica 3
(3, 3, 4),  -- "Heróis da Galáxia" recomendado para Carlos Oliveira com a característica 4
(4, 4, 1),  -- "Culinária para Todos" recomendado para Ana Pereira com a característica 1
(5, 5, 2),  -- "Aventuras no Deserto" recomendado para Paulo Santos com a característica 2
(6, 6, 3),  -- "Caminho para a Vitória" recomendado para Fernanda Lima com a característica 3
(7, 7, 1),  -- "Noite de Terror" recomendado para Ricardo Mota com a característica 1
(8, 8, 4),  -- "Segredos do Oceano" recomendado para Beatriz Almeida com a característica 4
(9, 9, 2),  -- "Cidade das Sombras" recomendado para Darci Silva com a característica 2
(10, 10, 3), -- "Detetives Urbanos" recomendado para Lucas Costa com a característica 3
(11, 11, 4), -- "Vida Selvagem" recomendado para Juliana Rocha com a característica 4
(12, 12, 1), -- "Saga dos Reinos" recomendado para Ricardo Silva com a característica 1
(13, 13, 2), -- "Mundos Perdidos" recomendado para Larissa Gomes com a característica 2
(14, 14, 3), -- "Expedição Polar" recomendado para Fernando Alves com a característica 3
(15, 15, 4), -- "O Último Imperador" recomendado para Sabrina Martins com a característica 4
(16, 16, 1), -- "Aventuras Subaquáticas" recomendado para Eduardo Pereira com a característica 1
(17, 17, 2), -- "Tecnologia do Futuro" recomendado para Isabela Oliveira com a característica 2
(18, 18, 3), -- "Estrada para a Liberdade" recomendado para Gabriel Souza com a característica 3
(19, 19, 4), -- "Guerra de Clãs" recomendado para Tatiane Lima com a característica 4
(20, 20, 1), -- "Mistérios da Mente" recomendado para Otavio Souza com a característica 1
(2, 21, 2),  -- "Mistério na Floresta" recomendado para usuário 21 (recomendado de tipo "semelhante") com a característica 2
(3, 22, 3),  -- "Heróis da Galáxia" recomendado para usuário 22 (recomendado de tipo "popular") com a característica 3
(4, 23, 4),  -- "Culinária para Todos" recomendado para usuário 23 (recomendado de tipo "novidade") com a característica 4
(5, 24, 1),  -- "Aventuras no Deserto" recomendado para usuário 24 (recomendado de tipo "nota_alta") com a característica 1
(6, 25, 2),  -- "Caminho para a Vitória" recomendado para usuário 25 (recomendado de tipo "gostos_semelhantes") com a característica 2
(7, 26, 3),  -- "Noite de Terror" recomendado para usuário 26 (recomendado de tipo "filmes_populares") com a característica 3
(8, 27, 4),  -- "Segredos do Oceano" recomendado para usuário 27 (recomendado de tipo "semelhante") com a característica 4
(9, 28, 1),  -- "Cidade das Sombras" recomendado para usuário 28 (recomendado de tipo "popular") com a característica 1
(10, 29, 2), -- "Detetives Urbanos" recomendado para usuário 29 (recomendado de tipo "novidade") com a característica 2
(11, 30, 3), -- "Vida Selvagem" recomendado para usuário 30 (recomendado de tipo "nota_alta") com a característica 3
(12, 31, 4), -- "Saga dos Reinos" recomendado para usuário 31 (recomendado de tipo "gostos_semelhantes") com a característica 4
(13, 32, 1), -- "Mundos Perdidos" recomendado para usuário 32 (recomendado de tipo "filmes_populares") com a característica 1
(14, 33, 2), -- "Expedição Polar" recomendado para usuário 33 (recomendado de tipo "semelhante") com a característica 2
(15, 34, 3), -- "O Último Imperador" recomendado para usuário 34 (recomendado de tipo "popular") com a característica 3
(16, 35, 4); -- "Aventuras Subaquáticas" recomendado para usuário 35 (recomendado de tipo "novidade") com a característica 4

#select avg(a.nota) as media_avaliacao , titulo_midia from tb_midias 
#inner join tb_avaliacao a on midia_id = id_midia
#group by midia_id
#order by media_avaliacao desc
#limit 5;

#select c.descricao , count(c.descricao) as caracts from tb_midia_caract
#inner join tb_midias on midia_id = id_midia
#inner join tb_caracteristicas c on caracteristica_id = id_caracteristica
#group by caracteristica_id 
#order by caracts desc
#limit 5;

#select sum(h.duracao) as tempo_assistido , titulo_midia, tipo from tb_historico h
#inner join tb_midias on midia_id = id_midia 
#where tipo = 'filme'
#group by midia_id
#order by tempo_assistido
#desc limit 3;

#select sum(h.duracao) as tempo_assistido , u.nome from tb_historico h
#inner join tb_usuarios u on usuario_id = id_usuario
#group by usuario_id
#order by tempo_assistido
#desc

#select descricao_genero as sexo , sum(duracao) as Contagem from tb_historico 
#inner join tb_usuarios on usuario_id = id_usuario
#inner join tb_generos on genero_id = id_genero 
#group by genero_id
#order by Contagem desc
#limit 1;

#select id_caracteristica , c.descricao from tb_caracteristicas c
#left join tb_midia_caract on caracteristica_id = id_caracteristica
#where caracteristica_id is null
#order by c.descricao

#select genero_id, max(a.nota) as nota_maxima,
     #descricao_genero as genero ,
     #(select c.descricao
     #from tb_caracteristicas c
     #inner join tb_avaliacao  on caracteristica_id = id_caracteristica
     #where nota = max(a.nota) 
     #and genero_id = genero_id
     #LIMIT 1) AS caracteristica_descricao
#from tb_avaliacao a

#inner join tb_usuarios  ON usuario_id = id_usuario
#inner join tb_midias  ON midia_id = id_midia
#inner join tb_generos  ON genero_id = id_genero
#inner join tb_caracteristicas c ON caracteristica_id = id_caracteristica
    
#group by genero_id, genero 
#order by nota_maxima desc;

#select  tipo_recomendacao , titulo_midia , u.nome , avg(nota_media_recomendada) as nota from tb_recomendados 
#inner join tb_usuarios u on usuario_id = id_usuario
#inner join tb_recomendados_midia on recomendados_id = id_recomendados
#inner join tb_midias on midia_id = id_midia
#group by recomendados_id, usuario_id , tipo_recomendacao , titulo_midia
#order by nota desc 


