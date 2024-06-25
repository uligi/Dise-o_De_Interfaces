# Dise-o_De_Interfaces

-- Crear la base de datos
CREATE DATABASE SolucionesPHDB;
GO

-- Usar la base de datos creada
USE SolucionesPHDB;
GO

-- Tabla de Roles
CREATE TABLE Roles (
    RolID INT PRIMARY KEY IDENTITY(1,1),
    NombreRol NVARCHAR(50) NOT NULL
);

-- Insertar datos en la tabla de Roles
INSERT INTO Roles (NombreRol) VALUES ('Admin'), ('Empleado'), ('Cliente');

-- Tabla de Usuarios (Para Administradores, Empleados y Clientes)
CREATE TABLE Usuarios (
    UsuarioID INT PRIMARY KEY IDENTITY(1,1),
    NombreUsuario NVARCHAR(100) NOT NULL UNIQUE,
    Contrasena NVARCHAR(255) NOT NULL,
    Nombre NVARCHAR(100) NOT NULL,
    Correo NVARCHAR(100) NOT NULL,
    Telefono NVARCHAR(20),
    Direccion NVARCHAR(255),
    RolID INT NOT NULL,
    FechaCreacion DATETIME DEFAULT GETDATE(),
    FechaActualizacion DATETIME DEFAULT GETDATE(),
    FOREIGN KEY (RolID) REFERENCES Roles(RolID)
);

-- Tabla de Productos
CREATE TABLE Productos (
    ProductoID INT PRIMARY KEY IDENTITY(1,1),
    Nombre NVARCHAR(100) NOT NULL,
    Descripcion NVARCHAR(500),
    Precio DECIMAL(10, 2) NOT NULL,
    Cantidad INT NOT NULL,
    UrlImagen NVARCHAR(255),
    FechaCreacion DATETIME DEFAULT GETDATE(),
    FechaActualizacion DATETIME DEFAULT GETDATE()
);

-- Tabla de Pedidos
CREATE TABLE Pedidos (
    PedidoID INT PRIMARY KEY IDENTITY(1,1),
    ClienteID INT NOT NULL,
    EstadoPedido NVARCHAR(50) NOT NULL,
    FechaPedido DATETIME DEFAULT GETDATE(),
    FOREIGN KEY (ClienteID) REFERENCES Usuarios(UsuarioID)
);

-- Tabla de Detalles de Pedido
CREATE TABLE DetallesPedido (
    DetallePedidoID INT PRIMARY KEY IDENTITY(1,1),
    PedidoID INT NOT NULL,
    ProductoID INT NOT NULL,
    Cantidad INT NOT NULL,
    Precio DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (PedidoID) REFERENCES Pedidos(PedidoID),
    FOREIGN KEY (ProductoID) REFERENCES Productos(ProductoID)
);

-- Tabla de Inventarios
CREATE TABLE Inventarios (
    InventarioID INT PRIMARY KEY IDENTITY(1,1),
    ProductoID INT NOT NULL,
    Cantidad INT NOT NULL,
    Ubicacion NVARCHAR(100),
    FOREIGN KEY (ProductoID) REFERENCES Productos(ProductoID)
);

-- Datos de ejemplo para Usuarios (Administradores, Empleados y Clientes)
INSERT INTO Usuarios (NombreUsuario, Contrasena, Nombre, Correo, Telefono, Direccion, RolID)
VALUES 
('admin', 'password1', 'Administrador', 'admin@example.com', '123456789', 'Calle Falsa 123', 1),
('empleado1', 'password2', 'Empleado Uno', 'empleado1@example.com', '987654321', 'Avenida Siempre Viva 456', 2),
('cliente1', 'password3', 'Cliente Uno', 'cliente1@example.com', '555555555', 'Calle 3', 3);
