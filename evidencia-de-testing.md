# 🧪 Evidencia de Testing y Artefactos QA

Este documento contiene evidencia de mi trabajo como QA Tester, incluyendo diseño de casos de prueba, reportes de bugs y consultas SQL para validación de datos.

---

## 1. Casos de Prueba (Test Cases)

**Proyecto:** App móvil de repartidores (Entorno: Emulador Android Studio, Android 9.0)

| ID | Nombre del caso de prueba | Precondiciones | Pasos | Resultado Esperado | Estado |
| :--- | :--- | :--- | :--- | :--- | :--- |
| TC-01 | Aceptación del pedido | Usuario logueado como repartidor sin pedidos. | 1. Tocar botón "Aceptar" en la tarjeta.<br>2. Tocar "Sí" en la notificación. | 1. Tarjeta se mueve a la derecha.<br>2. Punto azul en "Mis pedidos".<br>3. Botón cambia a "Completar". | ❌ FAIL (Ver bug-112) |

---

## 2. Pruebas de API (API Testing)

**Endpoint:** `POST /api/v1/courier` (Crear repartidor)

| Caso | Descripción de la validación | Resultado Esperado | Estado |
| :--- | :--- | :--- | :--- |
| API-01 | Crear repartidor con letras latinas (longitud 2, 5, 10). | 201 Created | ✅ PASS |
| API-02 | Crear repartidor con caracteres especiales/números. | 400 Bad Request | ✅ PASS |
| API-03 | Crear repartidor con campos en blanco. | 400 Bad Request | ✅ PASS |
| API-04 | Validar que el repartidor se guarde correctamente en la BD. | Registro en tabla Repartidores. | ❌ FAIL (Ver bug-111) |

---

## 3. Reporte de Bugs (Bug Reports)

*Nota: Los siguientes son ejemplos de reportes basados en los fallos detectados durante las pruebas.*

**ID:** bug-112
**Título:** [Mis Pedidos] La tarjeta de pedido no cambia a "Completar" después de aceptar el pedido.
**Severidad:** Alta
**Prioridad:** Alta
**Entorno:** Emulador Android 9.0.
**Pasos para reproducir:** 
1. Iniciar sesión como repartidor sin pedidos.
2. Tocar el botón "Aceptar" en la tarjeta de pedido.
3. Confirmar en la notificación emergente.
**Resultado Esperado:** La tarjeta se mueve, aparece el punto azul y el botón cambia a "Completar".
**Resultado Actual:** La tarjeta se mueve, pero el botón sigue diciendo "Aceptar".
**Evidencia:** (Adjuntar captura de pantalla aquí).

---

## 4. Validación de Datos con SQL

**Contexto:** El servicio de soporte reportó que los pedidos con "arroz" tardan más de 30 minutos. Se realizó la siguiente consulta para validar la queja.

```sql
-- Consulta para encontrar pedidos con arroz que tardaron más de 30 min
SELECT o.ORDERS_ID, o.EMPLOYEE_ID
FROM Pedidos o
WHERE o.ITEMS LIKE '%arroz%' 
  AND o.DELIVERY_TIME > 30;