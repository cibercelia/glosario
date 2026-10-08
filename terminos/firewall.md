---
title: "Firewall (cortafuegos)"
category: "Redes y seguridad perimetral"
author: "@empoleon00"
tags:
  - firewall
  - redes
  - filtrado-de-trafico
  - seguridad-perimetral
summary: "Sistema que controla el tráfico de red entrante y saliente mediante reglas para permitir o bloquear conexiones según una política de seguridad."
---

# Firewall (cortafuegos)

<div class="term-meta-box">
  <div class="term-meta-item">
    <span class="term-meta-label">Categoría</span>
    <span class="term-meta-value">Redes y seguridad perimetral</span>
  </div>
  <div class="term-meta-item">
    <span class="term-meta-label">Autor / Colaborador</span>
    <span class="term-meta-value"><a href="https://github.com/empoleon00" target="_blank">@empoleon00</a></span>
  </div>
</div>

## 📖 Definición

Un **firewall** o **cortafuegos** es un sistema de hardware, software o ambos que supervisa y controla el tráfico de red que entra y sale de un dispositivo o una red. Aplica reglas definidas por una política de seguridad para permitir, rechazar o registrar las comunicaciones.

Puede proteger un equipo individual (firewall de host) o una red completa (firewall de red). Según su tecnología, puede filtrar paquetes por direcciones y puertos, mantener el estado de las conexiones o inspeccionar protocolos y contenido de aplicación. Los firewalls de nueva generación (NGFW) pueden añadir funciones como prevención de intrusiones y reconocimiento de aplicaciones.

!!! warning "Importante"
    Un firewall reduce la superficie de exposición, pero no reemplaza las actualizaciones, la autenticación segura ni la protección de los servicios permitidos. Una regla demasiado amplia puede dejar accesibles sistemas que deberían estar restringidos.

---

## ⚙️ ¿Cómo funciona? / Principios fundamentales

1. **Observa el tráfico**: examina datos como las direcciones IP de origen y destino, el protocolo, los puertos y, según el tipo de firewall, el estado de la conexión o información de la capa de aplicación.
2. **Compara con las reglas**: evalúa las reglas en el orden y con la prioridad definidos por el producto. La política debe especificar qué tráfico se permite, cuál se bloquea y qué eventos se registran.
3. **Aplica la decisión**: acepta, descarta o rechaza la comunicación y, cuando corresponde, genera registros para auditoría y detección de incidentes.

En un firewall con seguimiento de estado (*stateful*), las respuestas a conexiones iniciadas desde dentro suelen reconocerse como parte de una sesión válida. Un filtro sin estado evalúa cada paquete de forma independiente.

---

## 🎯 Ejemplo práctico o escenario de demostración

En un servidor Linux, una política de mínimo privilegio debe bloquear por defecto las conexiones entrantes y abrir únicamente los servicios necesarios.

=== "Escenario vulnerable / configuración permisiva"

    ```bash
    # Regla excesivamente amplia que expone todos los puertos TCP
    sudo ufw allow 1:65535/tcp
    ```

    Una configuración como esta expone prácticamente todos los puertos TCP a cualquier origen, anulando la protección perimetral del equipo.

=== "Escenario seguro / configuración restringida (UFW)"

    ```bash
    # Política por defecto: denegar tráfico entrante y permitir saliente
    sudo ufw default deny incoming
    sudo ufw default allow outgoing

    # Permitir SSH solo desde una subred administrativa interna
    sudo ufw allow from 192.0.2.0/24 to any port 22 proto tcp

    # Permitir tráfico web público estrictamente necesario
    sudo ufw allow 80/tcp
    sudo ufw allow 443/tcp

    # Activar y verificar estado del cortafuegos
    sudo ufw enable
    sudo ufw status numbered
    ```

    La subred `192.0.2.0/24` es un rango reservado para documentación; debe sustituirse por la subred administrativa real. Se recomienda confirmar el acceso SSH antes de activar UFW para evitar bloqueos accidentales.

---

## 🛡️ Medidas de mitigación y buenas prácticas

- [x] **Aplicar denegación por defecto**: aplicar el principio de denegación por defecto y permitir únicamente los flujos y servicios estrictamente necesarios.
- [x] **Restringir por origen y destino**: acotar las reglas por dirección IP de origen, destino, protocolo y puerto siempre que sea viable.
- [x] **Revisar y auditar reglas periódicamente**: eliminar reglas obsoletas y documentar el motivo y el responsable de cada excepción.
- [x] **Registrar y supervisar eventos**: almacenar registros de tráfico relevante y analizarlos para detectar anomalías o intentos de intrusión.
- [x] **Mantener el software actualizado**: aplicar parches al cortafuegos y validar los cambios antes de su despliegue en producción.
- [x] **Defensa en profundidad**: usar cortafuegos de host y de red como capas complementarias sin asumir que uno sustituye al otro.

---

## 🔗 Referencias y enlaces de interés

- [NIST SP 800-41 Rev. 1: Guidelines on Firewalls and Firewall Policy](https://csrc.nist.gov/pubs/sp/800/41/r1/final)
- [Documentación de UFW en Ubuntu](https://help.ubuntu.com/community/UFW)
- [Documentación de nftables](https://www.netfilter.org/projects/nftables/)
