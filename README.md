

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8 */;

--
-- Database: `kripton`
--

DELIMITER $$
--
-- Procedures
--
CREATE DEFINER=`root`@`localhost` PROCEDURE `adicionar_usuario`(
  IN p_nome VARCHAR(20),
  IN p_sobrenome VARCHAR(20),
  IN p_data_nascimento DATE,
  IN p_email VARCHAR(30),
  IN p_senha VARCHAR(20),
  IN p_foto VARCHAR(20),
  IN p_nacionalidade VARCHAR(20)
)
BEGIN
  INSERT INTO users (nome, sobrenome, data_nascimento, email, senha, foto, nacionalidade)
  VALUES (p_nome, p_sobrenome, p_data_nascimento, p_email, p_senha, p_foto, p_nacionalidade);
END$$

--
-- Functions
--
CREATE DEFINER=`root`@`localhost` FUNCTION `atualiza_curtidas_descurtidas`() RETURNS tinyint(1)
BEGIN
    DECLARE total_curtidas INT;
    DECLARE total_descurtidas INT;
    
    SELECT SUM(curtidas) INTO total_curtidas FROM comentario;
    SELECT SUM(descurtidas) INTO total_descurtidas FROM comentario;

    UPDATE comentario SET curtidas = total_curtidas, descurtidas = total_descurtidas;
    
    RETURN TRUE;
END$$

DELIMITER ;

-- --------------------------------------------------------

--
-- Estrutura da tabela `chat`
--

CREATE TABLE IF NOT EXISTS `chat` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `data` date DEFAULT NULL,
  `id_de` int(11) DEFAULT NULL,
  `id_para` int(11) DEFAULT NULL,
  `mensagem` varchar(1000) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;

--
-- Acionadores `chat`
--
DROP TRIGGER IF EXISTS `chat_insert_trigger`;
DELIMITER //
CREATE TRIGGER `chat_insert_trigger` AFTER INSERT ON `chat`
 FOR EACH ROW BEGIN
    INSERT INTO notificacao (notificacao) VALUES (NEW.mensagem);
END
//
DELIMITER ;

-- --------------------------------------------------------

--
-- Estrutura da tabela `comentarios`
--

CREATE TABLE IF NOT EXISTS `comentarios` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `data` date DEFAULT NULL,
  `comentario` varchar(2000) DEFAULT NULL,
  `id_user_de` int(11) DEFAULT NULL,
  `id_user_para` int(11) DEFAULT NULL,
  `curtida` int(11) DEFAULT NULL,
  `descurtida` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;

-- --------------------------------------------------------

--
-- Estrutura da tabela `curtidas`
--

CREATE TABLE IF NOT EXISTS `curtidas` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `data` date DEFAULT NULL,
  `id_post` int(11) DEFAULT NULL,
  `id_comentario` int(11) DEFAULT NULL,
  `id_curtida` int(11) DEFAULT NULL,
  `id_descurtida` int(11) DEFAULT NULL,
  `id_para` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;

-- --------------------------------------------------------

--
-- Estrutura da tabela `momentos_bt`
--

CREATE TABLE IF NOT EXISTS `momentos_bt` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `data` date DEFAULT NULL,
  `id_para` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;

-- --------------------------------------------------------

--
-- Estrutura da tabela `news`
--

CREATE TABLE IF NOT EXISTS `news` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `data` date DEFAULT NULL,
  `titulo` varchar(20) DEFAULT NULL,
  `descricao` varchar(1000) DEFAULT NULL,
  `origem` varchar(10) DEFAULT NULL,
  `id_user` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `id_user` (`id_user`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;

-- --------------------------------------------------------

--
-- Estrutura da tabela `notificacao`
--

CREATE TABLE IF NOT EXISTS `notificacao` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `id_de` int(11) DEFAULT NULL,
  `id_para` int(11) DEFAULT NULL,
  `titulo` varchar(100) NOT NULL,
  `notificacao` varchar(256) DEFAULT NULL,
  `data` datetime DEFAULT CURRENT_TIMESTAMP,
  `alerta` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8 AUTO_INCREMENT=7 ;

--
-- Extraindo dados da tabela `notificacao`
--

INSERT INTO `notificacao` (`id`, `id_de`, `id_para`, `titulo`, `notificacao`, `data`, `alerta`) VALUES
(6, 1, 2, 'Nova Mensagem', 'Olá Marcilei', '2023-05-10 19:39:44', NULL);

-- --------------------------------------------------------

--
-- Stand-in structure for view `notificacao_`
--
CREATE TABLE IF NOT EXISTS `notificacao_` (
`notificacao` varchar(256)
,`titulo` varchar(100)
,`id_de` int(11)
,`id_para` int(11)
,`id` int(11)
,`data` datetime
);
-- --------------------------------------------------------

--
-- Estrutura da tabela `post`
--

CREATE TABLE IF NOT EXISTS `post` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `data` date DEFAULT NULL,
  `publicacao` varchar(2000) DEFAULT NULL,
  `id_user_de` int(11) DEFAULT NULL,
  `id_user_para` int(11) DEFAULT NULL,
  `curtida` int(11) DEFAULT NULL,
  `descurtida` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;

-- --------------------------------------------------------

--
-- Estrutura da tabela `seguidores`
--

CREATE TABLE IF NOT EXISTS `seguidores` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `amigos` int(11) DEFAULT NULL,
  `id_user_de` int(11) DEFAULT NULL,
  `id_user_para` int(11) DEFAULT NULL,
  `id_user` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `id_user` (`id_user`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;

-- --------------------------------------------------------

--
-- Estrutura da tabela `users`
--

CREATE TABLE IF NOT EXISTS `users` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `nome` varchar(20) DEFAULT NULL,
  `sobrenome` varchar(20) DEFAULT NULL,
  `data_nascimento` date DEFAULT NULL,
  `idade` varchar(5) DEFAULT NULL,
  `email` varchar(30) DEFAULT NULL,
  `senha` varchar(20) DEFAULT NULL,
  `foto` varchar(20) DEFAULT NULL,
  `nacionalidade` varchar(20) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;

-- --------------------------------------------------------

--
-- Structure for view `notificacao_`
--
DROP TABLE IF EXISTS `notificacao_`;

CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL SECURITY DEFINER VIEW `notificacao_` AS select `n`.`notificacao` AS `notificacao`,`n`.`titulo` AS `titulo`,`n`.`id_de` AS `id_de`,`n`.`id_para` AS `id_para`,`n`.`id` AS `id`,`n`.`data` AS `data` from `notificacao` `n` order by `n`.`notificacao`;

--
-- Constraints for dumped tables
--

--
-- Limitadores para a tabela `news`
--
ALTER TABLE `news`
  ADD CONSTRAINT `news_ibfk_1` FOREIGN KEY (`id_user`) REFERENCES `users` (`id`);

--
-- Limitadores para a tabela `seguidores`
--
ALTER TABLE `seguidores`
  ADD CONSTRAINT `seguidores_ibfk_1` FOREIGN KEY (`id_user`) REFERENCES `users` (`id`);

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
