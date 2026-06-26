# Docker-ERPNext

ERPNext: Sistema ERP empresarial completo y autohospedado en Docker.

## ¿Qué es ERPNext?

ERPNext es un sistema ERP (Enterprise Resource Planning) profesional, completo y open source diseñado para gestionar todos los aspectos de un negocio: contabilidad, inventario, ventas, compras, recursos humanos, proyectos, manufactura, y más. A diferencia de SAP o Oracle (costosísimos), ERPNext es gratuito, modular, y se instala en tu servidor en minutos con Docker.

### Stack profesional
Construido sobre Frappe Framework (Python + JavaScript), usa MariaDB/PostgreSQL, interfaz web moderna, API REST completa, permisos granulares, workflows, customizaciones sin código. Desde startups hasta empresas medianas, ERPNext maneja contabilidad, inventario, CRM, RRHH, proyectos, manufactura. Todo en un único sistema integrado bajo control total.

### Control total de datos
Tu ERP en tu servidor. Sin cuotas mensuales, sin vendor lock-in, sin datos en servidores remotos. Completamente gratis y open source. Comunidad de 500K+ usuarios.

## Módulos principales

### Contabilidad
Facturas, asientos, reportes financieros, auditoría, multi-moneda.

### Inventario
Stock, almacenes, movimientos, valuación, alertas.

### CRM
Clientes, leads, oportunidades, seguimiento.

### Ventas
Presupuestos, órdenes, facturas, entregas.

### Compras
Órdenes de compra, proveedores, recepción, gastos.

### RRHH
Empleados, asistencias, nómina, permisos, evaluaciones.

### Proyectos
Tareas, presupuestos, timesheets, facturas.

### Manufactura
Órdenes producción, BOM, material consumption, subcontratos.

### Activos
Gestión de equipos, depreciación, mantenimiento.

### Calidad
Estándares QC, inspecciones, no conformidades.

### Workflows
Flujos personalizados, aprobaciones, automaciones.

### Permisos granulares
RBAC, permisos por documento, vistas personalizadas.

## Requisitos del sistema

- Docker & Docker Compose
- 8-16 GB RAM mínimo (ERP requiere recursos)
- 4+ CPU cores
- 50+ GB espacio disco (BD, datos, documentos)
- MariaDB 10DB 10.3+ o PostgreSQL 12+ (incluido)
- Puertos: 80/443 (web), 3306 (BD)

**Importante**: ERPNext es más pesado que aplicaciones típicas. ERP con muchos usuarios requiere recursos. Mínimo recomendado 8GB RAM para producción.

## Instalación con Docker

### Opción 1: Instalación rápida

```bash
git clone https://github.com/frappe/frappe_docker.git
cd frappe_docker
cp env-example .env
docker compose up -d
```

### Opción 2: Setup manual

Crea un archivo `docker-compose.yml` con el siguiente contenido:

```yaml
version: '3.8'

services:
  mariadb:
    image: mariadb:10.6
    container_name: erpnext_db
    restart: unless-stopped
    environment:
      - MYSQL_ROOT_PASSWORD=contraseña-root
      - MYSQL_DATABASE=erpnext
    volumes:
      - mariadb_data:/var/lib/mysql

  redis:
    image: redis:7
    container_name: erpnext_redis
    restart: unless-stopped

  erpnext:
    image: frappe/erpnext:latest
    container_name: erpnext
    restart: unless-stopped
    ports:
      - "80:8000"
      - "443:8000"
    environment:
      - DB_HOST=mariadb
      - DB_PORT=3306
      - DB_NAME=erpnext
      - REDIS_CACHE=redis:6379
    depends_on:
      - mariadb
      - redis

volumes:
  mariadb_data:
```

### Iniciar

```bash
docker compose up -d
```

### Setup inicial (primera vez)

```bash
docker compose exec erpnext bench new-site erpnext.localhost
docker compose exec erpnext bench install-app erpnext
```

### Acceder

http://localhost con usuario: admin (contraseña generada al setup)

## Primeros pasos

### 1. Setup inicial
- Accede a http://localhost
- Wizard te pide: nombre empresa, región, moneda, email admin
- Se crea la BD y loguea automáticamente

### 2. Dashboard principal
- Ves home con widgets de últimas transacciones
- Sidebar izquierdo con todos los módulos
- Búsqueda global arriba para cualquier documento

### 3. Crear datos maestros
- Setup → Settings: empresa, datos, moneda
- CRM → Customers: crea clientes
- Buying → Suppliers: agrega proveedores
- Stock → Item: crea productos/servicios

### 4. Hacer primera transacción
- Selling → Sales Order → New
- Selecciona cliente, agrega items, cantidades, precios
- Submit (guardar)
- Se pueden generar entregas y facturas automáticamente

### 5. Ver reportes
- Accounting → Reports → Trial Balance, P&L, etc
- Stock → Reports → Stock Balance
- HR → Attendance Report

## Casos de uso

- **PyME manufacturera**: Gestiona producción, inventario, ventas desde un lugar
- **Distribuidor**: Multi-almacenes, control de stock, facturas
- **Servicios**: Proyectos, timesheets, facturas a clientes
- **Retail**: Punto de venta integrado, inventario
- **Control financiero**: Contabilidad completa, auditoría, reportes
- **RRHH centralizado**: Nómina, asistencia, evaluaciones

## Gestión y mantenimiento

### Ver logs
```bash
docker logs -f erpnext
```

### Backup automático
```bash
docker exec erpnext bench backup
```

### Actualizar ERPNext
```bash
docker compose pull
docker compose exec erpnext bench migrate
docker compose up -d
```

### Crear nuevo sitio
```bash
docker compose exec erpnext bench new-site otraempresa.localhost
```

### Acceso a base de datos
```bash
docker exec -it erpnext_db mysql -u root -p erpnext
```

### Reiniciar servicios
```bash
docker compose restart
```

## HTTPS con Caddy

### Configurar para producción
```
erpnext.tudominio.com {
    reverse_proxy localhost:8000
}
```

### Multi-sitio con subdominio
```
empresa1.tudominio.com {
    reverse_proxy localhost:8000
}

empresa2.tudominio.com {
    reverse_proxy localhost:8000
}
```

## Licencia

ERPNext se distribuye bajo la Licencia [GNU General Public License v3.0](https://github.com/frappe/erpnext/blob/develop/LICENSE.md). Este repositorio contiene únicamente archivos de configuración (`docker-compose.yml`, `README.md`) y no redistribuye el código fuente de la aplicación. Al usar esta imagen, estás sujeto a los términos de la licencia del proyecto original.

> **Nota de responsabilidad**: Este README y los archivos de configuración se proporcionan tal cual. Es responsabilidad del usuario asegurar la seguridad de su despliegue (por ejemplo, cambiar contraseñas predeterminadas, mantener actualizadas las imágenes de Docker, etc.). Siempre sigue las mejores prácticas de seguridad para tu entorno específico (hogar, oficina, nube, etc.).