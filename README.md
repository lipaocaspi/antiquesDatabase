# AntiquesDB

#### DER

![DER](https://raw.githubusercontent.com/lipaocaspi/AntiquesDB/main/DER.png)



#### DDL

```sql
SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema antiquesDB
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema antiquesDB
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `antiquesDB` DEFAULT CHARACTER SET utf8 ;
USE `antiquesDB` ;

-- -----------------------------------------------------
-- Table `antiquesDB`.`user`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `antiquesDB`.`user` (
  `id_user` VARCHAR(15) NOT NULL,
  `name_user` VARCHAR(20) NOT NULL,
  `password_user` VARCHAR(45) NOT NULL,
  `fullname_user` VARCHAR(45) NOT NULL,
  `dateofregistration_user` DATE NOT NULL,
  `role_user` ENUM('Vendedor', 'Comprador', 'Administrador') NOT NULL,
  `phone_user` VARCHAR(20) NOT NULL,
  PRIMARY KEY (`id_user`),
  UNIQUE INDEX `id_user_UNIQUE` (`id_user` ASC) VISIBLE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `antiquesDB`.`country`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `antiquesDB`.`country` (
  `id_country` VARCHAR(5) NOT NULL,
  `name_country` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id_country`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `antiquesDB`.`region`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `antiquesDB`.`region` (
  `id_region` VARCHAR(5) NOT NULL,
  `name_region` VARCHAR(45) NOT NULL,
  `id_country` VARCHAR(5) NOT NULL,
  PRIMARY KEY (`id_region`),
  INDEX `fk_region_country_idx` (`id_country` ASC) VISIBLE,
  CONSTRAINT `fk_region_country`
    FOREIGN KEY (`id_country`)
    REFERENCES `antiquesDB`.`country` (`id_country`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
COMMENT = '		';


-- -----------------------------------------------------
-- Table `antiquesDB`.`city`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `antiquesDB`.`city` (
  `id_city` VARCHAR(5) NOT NULL,
  `name_city` VARCHAR(45) NOT NULL,
  `id_region` VARCHAR(5) NOT NULL,
  PRIMARY KEY (`id_city`),
  INDEX `fk_city_region1_idx` (`id_region` ASC) VISIBLE,
  CONSTRAINT `fk_city_region1`
    FOREIGN KEY (`id_region`)
    REFERENCES `antiquesDB`.`region` (`id_region`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `antiquesDB`.`address`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `antiquesDB`.`address` (
  `id_address` INT NOT NULL AUTO_INCREMENT,
  `detail_address` VARCHAR(50) NOT NULL,
  `postalcode_address` VARCHAR(10) NOT NULL,
  `id_city` VARCHAR(5) NOT NULL,
  `id_user` VARCHAR(15) NOT NULL,
  PRIMARY KEY (`id_address`),
  INDEX `fk_address_city1_idx` (`id_city` ASC) VISIBLE,
  INDEX `fk_address_user1_idx` (`id_user` ASC) VISIBLE,
  CONSTRAINT `fk_address_city1`
    FOREIGN KEY (`id_city`)
    REFERENCES `antiquesDB`.`city` (`id_city`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_address_user1`
    FOREIGN KEY (`id_user`)
    REFERENCES `antiquesDB`.`user` (`id_user`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `antiquesDB`.`category`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `antiquesDB`.`category` (
  `id_category` INT NOT NULL AUTO_INCREMENT,
  `name_category` VARCHAR(20) NOT NULL,
  `description_category` VARCHAR(100) NULL,
  PRIMARY KEY (`id_category`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `antiquesDB`.`antique`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `antiquesDB`.`antique` (
  `id_antique` INT NOT NULL AUTO_INCREMENT,
  `name_antique` VARCHAR(20) NOT NULL,
  `description_antique` VARCHAR(45) NOT NULL,
  `period_antique` VARCHAR(20) NOT NULL,
  `state_antique` ENUM('Excelente', 'Bueno', 'Regular') NOT NULL,
  `price_antique` DOUBLE NOT NULL,
  `picture_antique` VARCHAR(100) NOT NULL,
  `dateofsale_antique` DATE NOT NULL,
  `search_antique` INT NOT NULL DEFAULT 0,
  `id_category` INT NOT NULL,
  `id_country` VARCHAR(5) NOT NULL,
  `id_user` VARCHAR(15) NOT NULL,
  PRIMARY KEY (`id_antique`),
  UNIQUE INDEX `id_antique_UNIQUE` (`id_antique` ASC) VISIBLE,
  INDEX `fk_antique_category1_idx` (`id_category` ASC) VISIBLE,
  INDEX `fk_antique_country1_idx` (`id_country` ASC) VISIBLE,
  INDEX `fk_antique_user1_idx` (`id_user` ASC) VISIBLE,
  CONSTRAINT `fk_antique_category1`
    FOREIGN KEY (`id_category`)
    REFERENCES `antiquesDB`.`category` (`id_category`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_antique_country1`
    FOREIGN KEY (`id_country`)
    REFERENCES `antiquesDB`.`country` (`id_country`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_antique_user1`
    FOREIGN KEY (`id_user`)
    REFERENCES `antiquesDB`.`user` (`id_user`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `antiquesDB`.`payment_method`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `antiquesDB`.`payment_method` (
  `id_payment_method` INT NOT NULL AUTO_INCREMENT,
  `name_payment_method` VARCHAR(30) NOT NULL,
  PRIMARY KEY (`id_payment_method`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `antiquesDB`.`transaction`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `antiquesDB`.`transaction` (
  `id_transaction` INT NOT NULL AUTO_INCREMENT,
  `price_transaction` DOUBLE NOT NULL,
  `date_transaction` DATE NOT NULL,
  `status_transaction` ENUM('En proceso', 'Enviado', 'Entregado') NOT NULL,
  `id_buyer` VARCHAR(15) NOT NULL,
  `id_antique` INT NOT NULL,
  `id_payment_method` INT NOT NULL,
  PRIMARY KEY (`id_transaction`),
  INDEX `fk_transaction_user1_idx` (`id_buyer` ASC) VISIBLE,
  INDEX `fk_transaction_antique1_idx` (`id_antique` ASC) VISIBLE,
  INDEX `fk_transaction_payment_method1_idx` (`id_payment_method` ASC) VISIBLE,
  CONSTRAINT `fk_transaction_user1`
    FOREIGN KEY (`id_buyer`)
    REFERENCES `antiquesDB`.`user` (`id_user`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_transaction_antique1`
    FOREIGN KEY (`id_antique`)
    REFERENCES `antiquesDB`.`antique` (`id_antique`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_transaction_payment_method1`
    FOREIGN KEY (`id_payment_method`)
    REFERENCES `antiquesDB`.`payment_method` (`id_payment_method`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `antiquesDB`.`activity_log`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `antiquesDB`.`activity_log` (
  `id_activity_log` INT NOT NULL AUTO_INCREMENT,
  `typeactivity_activity_log` VARCHAR(50) NOT NULL,
  `date_activity_log` DATETIME NOT NULL,
  `id_user` VARCHAR(15) NOT NULL,
  PRIMARY KEY (`id_activity_log`),
  INDEX `fk_activity_log_user1_idx` (`id_user` ASC) VISIBLE,
  CONSTRAINT `fk_activity_log_user1`
    FOREIGN KEY (`id_user`)
    REFERENCES `antiquesDB`.`user` (`id_user`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `antiquesDB`.`inventory`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `antiquesDB`.`inventory` (
  `id_inventory` INT NOT NULL AUTO_INCREMENT,
  `stock_inventory` INT NOT NULL,
  `status_inventory` ENUM('Disponible', 'Vendido', 'Retirado') NOT NULL,
  `dateupdate_inventory` DATE NOT NULL,
  `id_antique` INT NOT NULL,
  PRIMARY KEY (`id_inventory`),
  INDEX `fk_inventory_antique1_idx` (`id_antique` ASC) VISIBLE,
  CONSTRAINT `fk_inventory_antique1`
    FOREIGN KEY (`id_antique`)
    REFERENCES `antiquesDB`.`antique` (`id_antique`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
```



#### DML

```sql
-- TABLA USER
INSERT INTO `antiquesDB`.`user` (`id_user`, `name_user`, `password_user`, `fullname_user`, `dateofregistration_user`, `role_user`, `phone_user`)
VALUES 
('U001', 'jdoe', 'password123', 'John Doe', '2023-01-15', 'Vendedor', '123-456-7890'),
('U002', 'mjane', 'pass456', 'Mary Jane', '2023-02-20', 'Comprador', '987-654-3210'),
('U003', 'admin', 'adminpass', 'Admin User', '2023-01-01', 'Administrador', '555-555-5555');

-- TABLA COUNTRY
INSERT INTO `antiquesDB`.`country` (`id_country`, `name_country`)
VALUES 
('C001', 'USA'),
('C002', 'France'),
('C003', 'Italy');

-- TABLA REGION
INSERT INTO `antiquesDB`.`region` (`id_region`, `name_region`, `id_country`)
VALUES 
('R001', 'California', 'C001'),
('R002', 'Provence', 'C002'),
('R003', 'Tuscany', 'C003');

-- TABLA CITY
INSERT INTO `antiquesDB`.`city` (`id_city`, `name_city`, `id_region`)
VALUES 
('CI001', 'Los Angeles', 'R001'),
('CI002', 'Marseille', 'R002'),
('CI003', 'Florence', 'R003');

-- TABLA ADDRESS
INSERT INTO `antiquesDB`.`address` (`detail_address`, `postalcode_address`, `id_city`, `id_user`)
VALUES 
('123 Main St', '90001', 'CI001', 'U001'),
('456 Rue de France', '13001', 'CI002', 'U002');

-- TABLA CATEGORY
INSERT INTO `antiquesDB`.`category` (`name_category`, `description_category`)
VALUES 
('Furniture', 'Antique furniture items.'),
('Art', 'Paintings, sculptures, and other artworks.'),
('Jewelry', 'Antique jewelry pieces.');

-- TABLA ANTIQUE
INSERT INTO `antiquesDB`.`antique` (`name_antique`, `description_antique`, `period_antique`, `state_antique`, `price_antique`, `picture_antique`, `dateofsale_antique`, `id_category`, `id_country`, `id_user`)
VALUES 
('Victorian Chair', 'A beautiful Victorian-era chair.', '19th Century', 'Bueno', 1200.00, 'victorian_chair.jpg', '2024-01-01', 1, 'C001', 'U001'),
('Renaissance Painting', 'A painting from the Renaissance period.', '16th Century', 'Excelente', 50000.00, 'renaissance_painting.jpg', '2024-02-01', 2, 'C003', 'U001');

-- TABLA PAYMENT_METHOD
INSERT INTO `antiquesDB`.`payment_method` (`name_payment_method`)
VALUES 
('Credit Card'),
('PayPal'),
('Bank Transfer');

-- TABLA TRANSACTION
INSERT INTO `antiquesDB`.`transaction` (`price_transaction`, `date_transaction`, `status_transaction`, `id_buyer`, `id_antique`, `id_payment_method`)
VALUES 
(1200.00, '2024-02-15', 'Entregado', 'U002', 1, 1),
(50000.00, '2024-03-05', 'Enviado', 'U002', 2, 2);

-- TABLA ACTIVITY_LOG
INSERT INTO `antiquesDB`.`activity_log` (`typeactivity_activity_log`, `date_activity_log`, `id_user`)
VALUES 
('User Registration', '2023-01-15 12:00:00', 'U001'),
('Item Listed for Sale', '2024-01-01 09:30:00', 'U001'),
('Purchase Completed', '2024-02-15 14:00:00', 'U002');

-- TABLA INVENTORY
INSERT INTO `antiquesDB`.`inventory` (`stock_inventory`, `status_inventory`, `dateupdate_inventory`, `id_antique`)
VALUES 
(1, 'Vendido', '2024-02-15', 1),
(1, 'Disponible', '2024-03-01', 2);
```



#### CONSULTAS

* Obtén una lista de todas las piezas antiguas que están actualmente disponibles para la venta, incluyendo el nombre de la pieza, su categoría, precio y estado de conservación.

```sql
SELECT 
    a.name_antique AS 'Nombre de la Pieza',
    c.name_category AS 'Categoría',
    a.price_antique AS 'Precio',
    a.state_antique AS 'Estado de Conservación'
FROM 
    antique a
JOIN 
    category c ON a.id_category = c.id_category
JOIN 
    inventory i ON a.id_antique = i.id_antique
WHERE 
    i.status_inventory = 'Disponible';
```

* Busca todas las antigüedades dentro de una categoría específica (por ejemplo, 'Muebles') que tengan un precio dentro de un rango determinado (por ejemplo, entre 500 y 2000 dólares).

```sql
SELECT 
    a.name_antique AS 'Nombre de la Pieza',
    c.name_category AS 'Categoría',
    a.price_antique AS 'Precio',
    a.state_antique AS 'Estado de Conservación'
FROM 
    antique a
JOIN 
    category c ON a.id_category = c.id_category
WHERE 
    c.name_category = 'Furniture'
    AND a.price_antique BETWEEN 500 AND 2000;
```

* Muestra todas las piezas antiguas que un cliente específico ha vendido, incluyendo la fecha de la venta, el precio de venta y el comprador.

```sql
SELECT 
    a.name_antique AS 'Nombre de la Pieza',
    t.date_transaction AS 'Fecha de Venta',
    t.price_transaction AS 'Precio de Venta',
    u_buyer.fullname_user AS 'Comprador'
FROM 
    antique a
JOIN 
    transaction t ON a.id_antique = t.id_antique
JOIN 
    user u_buyer ON t.id_buyer = u_buyer.id_user
WHERE 
    a.id_user = 'U001';
```

* Calcula el total de ventas realizadas en un período específico, por ejemplo, durante el último
  mes.

```sql
SELECT 
    SUM(t.price_transaction) AS 'Total de Ventas'
FROM 
    transaction t
WHERE 
    t.date_transaction BETWEEN DATE_SUB(CURDATE(), INTERVAL 1 MONTH) AND CURDATE();
```

* Identifica los clientes que han realizado la mayor cantidad de compras en la plataforma.

```sql
SELECT 
    u.fullname_user AS 'Cliente',
    u.name_user AS 'Usuario',
    COUNT(t.id_transaction) AS 'Cantidad de Compras'
FROM 
    transaction t
JOIN 
    user u ON t.id_buyer = u.id_user
GROUP BY 
    t.id_buyer
ORDER BY 
    COUNT(t.id_transaction) DESC;
```

* Muestra las piezas antiguas que han recibido la mayor cantidad de visitas o consultas por parte de los usuarios.

```sql
SELECT 
    a.name_antique AS 'Nombre de la Pieza',
    a.search_antique AS 'Cantidad de Consultas'
FROM 
    antique a
ORDER BY 
    a.search_antique DESC;
```

* Obtén una lista de todas las piezas antiguas que se han vendido dentro de un rango de fechas específico, incluyendo la información del vendedor y comprador.

```sql
SELECT 
    a.name_antique AS 'Nombre de la Pieza',
    t.date_transaction AS 'Fecha de Venta',
    t.price_transaction AS 'Precio de Venta',
    u_seller.fullname_user AS 'Vendedor',
    u_buyer.fullname_user AS 'Comprador'
FROM 
    antique a
JOIN 
    transaction t ON a.id_antique = t.id_antique
JOIN 
    user u_seller ON a.id_user = u_seller.id_user
JOIN 
    user u_buyer ON t.id_buyer = u_buyer.id_user
WHERE 
    t.date_transaction BETWEEN '2024-01-01' AND '2024-12-31';
```

* Genera un informe del inventario actual de antigüedades disponibles para la venta, mostrando la cantidad de artículos por categoría.

```sql
SELECT 
    c.name_category AS 'Categoría',
    COUNT(a.id_antique) AS 'Cantidad de Artículos'
FROM 
    antique a
JOIN 
    category c ON a.id_category = c.id_category
JOIN 
    inventory i ON a.id_antique = i.id_antique
WHERE 
    i.status_inventory = 'Disponible'
GROUP BY 
    c.name_category;
```

