# ERPNext – ERP Open Source con Docker

ERPNext es un sistema **ERP 100% Open Source** diseñado para ayudarte a gestionar tu negocio de forma integral: contabilidad, inventario, ventas, fabricación, activos y proyectos, todo desde una única plataforma.

---

## 🎯 Motivación

Gestionar un negocio implica manejar múltiples áreas: facturación, stock, personal, producción y tareas ad-hoc.  
Mientras que muchas soluciones venden cada módulo por separado, **ERPNext integra todas estas funcionalidades de forma gratuita y abierta**, permitiéndote mantener el control total de tus datos.

---

## ✨ Funcionalidades principales

### 📊 Contabilidad
- Gestión completa del flujo de caja.
- Registro de transacciones.
- Informes financieros detallados y análisis contable.

### 📦 Gestión de pedidos e inventario
- Control de niveles de stock.
- Gestión de clientes, proveedores y envíos.
- Órdenes de venta, compra y cumplimiento de pedidos.

### 🏭 Fabricación
- Planificación del ciclo de producción.
- Control del consumo de materiales.
- Subcontratación y planificación de capacidad.

### 🖥 Gestión de activos
- Seguimiento del ciclo de vida de los activos.
- Infraestructura IT, maquinaria y equipamiento.
- Gestión centralizada de activos empresariales.

### 📁 Proyectos
- Gestión de proyectos internos y externos.
- Seguimiento de tareas, hojas de tiempo e incidencias.
- Control de presupuestos y rentabilidad.

---

## 🐳 Despliegue con Docker Compose

Este repositorio utiliza imágenes oficiales de **Frappe / ERPNext** y levanta todos los servicios necesarios para su correcto funcionamiento mediante Docker Compose.

---

## 🚀 Puesta en marcha

1. Clona el repositorio:
```bash
git clone https://github.com/JLalib/docker-erpnext-.git
cd docker-erpnext
```

2. Levanta los servicios:
```bash
docker compose up -d
```

3. Espera a que se complete la creación del sitio (puede tardar varios minutos).

4. Accede a ERPNext desde tu navegador:
```
http://TU_IP:8200
```

---

## 🔐 Credenciales iniciales

- **Usuario:** Administrator
- **Contraseña:** admin

⚠️ Cambia la contraseña tras el primer acceso.

---

## 📁 Volúmenes persistentes

| Volumen              | Descripción                          |
|----------------------|--------------------------------------|
| db-data              | Datos de MariaDB                     |
| redis-cache-data     | Caché Redis                          |
| redis-queue-data     | Cola Redis                           |
| sites                | Datos y configuración de ERPNext     |
| logs                 | Logs del sistema                     |

---

## 🔧 Notas importantes

- Despliegue orientado a **laboratorios o pruebas**.
- Para producción se recomienda:
  - Proxy inverso
  - HTTPS
  - Backups periódicos
  - Variables de entorno seguras

---

## 📘 Recursos oficiales

- https://erpnext.com/
- https://github.com/frappe/erpnext
- https://docs.erpnext.com/

---

## 👤 Autor

README generado para **JLalib** siguiendo el método **README Pro GitHub**.

