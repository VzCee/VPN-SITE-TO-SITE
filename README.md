# Documentacion **VPN Site-To-Site**

# Implementación de Túnel VPN Site-to-Site con FortiGate (IPsec)

**Estudiante:** Cesar Valdez Elibo

**Matrícula:** 2024-2372

**Asignatura:** Seguridad Informática

**Plataforma de Simulación:** PNETLab 

**Link del video:** https://youtu.be/c1vP7ITo3ps

## 1. Descripción del Proyecto

Este proyecto consiste en el diseño y despliegue de una infraestructura de red segura que comunica dos sucursales remotas mediante un túnel **IPsec VPN**. La solución garantiza la tríada de la seguridad (Confidencialidad, Integridad y Disponibilidad) al cifrar el tráfico que viaja a través de una red pública simulada por un Router ISP.

## 2. Topología de Red y Direccionamiento

La red se segmentó utilizando la matrícula **2024-2372** como base para el direccionamiento IP, validando el control sobre el esquema de red.
<img width="566" height="244" alt="image" src="https://github.com/user-attachments/assets/01a396c4-fe6e-468f-a8b2-2952cebcbe3a" />
<img width="856" height="485" alt="image" src="https://github.com/user-attachments/assets/3c422cae-3d86-4671-98eb-c922dcd6ad9a" />

## 3. Configuración Técnica (CLI)

### Fase 1: Parámetros de Seguridad (IKE)

Se estableció una autenticación mediante **Pre-Shared Key (PSK)** para validar la identidad de los extremos antes de negociar el cifrado.

- **Comando Clave:** `set psksecret 20242372`
- **Objetivo:** Asegurar que solo los Gateways con la llave correcta puedan establecer el túnel.
<img width="1849" height="250" alt="image" src="https://github.com/user-attachments/assets/aab25135-9d68-4496-b8f9-a1be92fc4349" />

### Enrutamiento y Políticas de Firewall

Para que el tráfico fluya, se configuraron rutas estáticas apuntando a la interfaz virtual de la VPN y políticas de seguridad bidireccionales (`set action accept`). Sin estas reglas, el firewall aplicaría un **Implicit Deny**, bloqueando la comunicación.

<img width="600" height="824" alt="image" src="https://github.com/user-attachments/assets/7c840922-fa97-456d-993b-2de85341acca" />

## 4. Verificación de Resultados

### Estado del Túnel (IPsec SA)

Mediante el comando `get vpn ipsec tunnel details`, validamos que el túnel está **ESTABLISHED**.

- **Evidencia:** Presencia de contadores de paquetes `RX/TX` y generación de códigos `SPI` (Security Parameter Index), lo que confirma que el cifrado está operando en tiempo real.

<img width="687" height="730" alt="image" src="https://github.com/user-attachments/assets/26bfa7da-cbc8-4ec6-a755-a312212f06ce" />

### Prueba de Conectividad End-to-End (Trace-route)

Se ejecutó la prueba desde la **VPC-1** hacia la **VPC-2**:

**trace 10.23.72.10**

**Análisis del resultado:**

1. **Salto 1 (10.20.24.1):** Alcance exitoso del Gateway local.
2. **Salto 2 (23.72.2.2):** El paquete salta directamente a la IP pública remota, demostrando que el tráfico viajó de forma transparente y segura a través del túnel, omitiendo los nodos internos del ISP.
3. **Salto 3 (10.23.72.10):** Llegada exitosa al host de destino.
<img width="894" height="141" alt="image" src="https://github.com/user-attachments/assets/aaa9b83c-09fe-4120-bc03-1ac9cdb35d0e" />

## 5. Conclusiones

La implementación exitosa de este laboratorio demuestra el dominio en la configuración de firewalls de próxima generación (NGFW) y la capacidad de asegurar comunicaciones en entornos empresariales mediante protocolos estándares de la industria.

