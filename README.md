# ProyectoParadigmas
# Alien Survivor 2D

Proyecto académico de 3º de carrera desarrollado en **Unity**.  
Es un juego 2D tipo *Vampire Survivors*: controlas a un alienígena atrapado en un campo lleno de criaturas que aparecen en oleadas y te persiguen sin descanso.

El objetivo es **sobrevivir el máximo tiempo posible**, esquivando enemigos, recogiendo experiencia y mejorando al personaje mediante armas y mejoras pasivas.

---

## 🎮 Mecánicas principales

- **Movimiento del jugador** con teclado (foco en esquivar y posicionarse).
- **Ataques automáticos**: las armas disparan solas según su cooldown.
- **Experiencia y nivel**:
  - Al matar enemigos caen pickups de experiencia.
  - Al subir de nivel se abre un menú de mejoras.
- **Mejoras**:
  - Nuevas armas.
  - Mejoras pasivas (vida, daño, velocidad, etc.).
  - Curaciones y buffs temporales.
- **Oleadas de enemigos**:
  - Cada vez más densas y difíciles.
  - Enemigos con distintas estadísticas (vida, daño, velocidad).
- **Enemigos jefe**:
  - Aparecen tras cierto tiempo.
  - Mucha más vida y daño.
  - Sueltan recompensas especiales al morir.
- **Escenario “infinito”**:
  - Se reutilizan/se desplazan secciones del mapa para que no haya bordes visibles.
  - Obstáculos que bloquean el movimiento.
- **Game Over**:
  - Cuando la vida del jugador llega a 0.
  - Se muestra puntuación y estadísticas de la partida.

---

## ⌨️ Controles

- **WASD** o **Flechas** → mover al personaje.
- **Esc** → pausar la partida / abrir menú de pausa.
- El ataque es **automático**, no hay botón de disparo.

*(Los controles se pueden ajustar fácilmente en el Input System de Unity.)*

---

## 🧱 Arquitectura (resumen)

He intentado aplicar principios de **diseño de software** (SRP, SOLID) y algunos **patrones de diseño** en la organización del código.

### Clases principales

- `GameManager`  
  Gestiona el ciclo de la partida (inicio, pausa, fin), tiempo, puntuación y referencia a los sistemas principales.

- `PlayerController`  
  Punto central del jugador. Conecta:
  - `PlayerMovement` (movimiento físico),
  - `PlayerHealth` (vida y daño),
  - `PlayerExperience` (experiencia y niveles),
  - `PlayerWeaponManager` (armas equipadas),
  - `PlayerAnimator` (animaciones del jugador).

- `EnemyBase` / `BossEnemy`  
  Lógica común de enemigos:
  - Movimiento hacia el jugador (o patrones específicos).
  - Daño por contacto.
  - Muerte y generación de experiencia.
  - En el caso de `BossEnemy`, se añade una máquina de estados para fases y ataques especiales.

- `WeaponBase` y derivadas (`ProjectileWeapon`, `AreaWeapon`, etc.)  
  Armas con ataque automático y cooldown.  
  Se instancian proyectiles o áreas de daño usando un sistema de **Object Pooling** para optimizar rendimiento.

- `UpgradeManager`  
  Muestra las opciones de mejora al subir de nivel y aplica la mejora seleccionada al jugador o a sus armas.

- `WaveManager`  
  Controla el ritmo de oleadas, el tipo y la cantidad de enemigos que aparecen y el momento en que entra un jefe.

- `UIManager`  
  Único punto de conexión entre la lógica del juego y la interfaz:
  - Pantalla principal.
  - HUD (vida, experiencia, nivel, tiempo, puntuación).
  - Menú de pausa.
  - Pantalla de Game Over.
  - Menú de opciones (volumen, etc.).

### Patrones y principios usados

- **Singleton / Facade**:  
  `GameManager`, `AudioManager`, `UIManager` actúan como fachada para simplificar el acceso a sistemas globales.

- **Strategy**:  
  - Diferentes comportamientos de movimiento de enemigos (`IEnemyMovementBehavior`).
  - Diferentes tipos de armas a través de `WeaponBase`.

- **State**:  
  - Máquina de estados para jefes (`BossState`, `BossStateMachine`).

- **Observer / Event-driven**:  
  - Sistema de eventos (`GameEvents`) para comunicar cambios de vida, experiencia, nivel, score, etc., a la UI.

- **Object Pooling**:  
  - `ObjectPoolManager` para reutilizar proyectiles, enemigos y pickups y evitar instanciar/destruir todo el rato.

---

## 🗂️ Estructura básica del proyecto

*(Los nombres de carpetas pueden variar un poco, pero la idea es esta.)*

- `Assets/Scripts/Player`  
  `PlayerController`, `PlayerMovement`, `PlayerHealth`, `PlayerExperience`, `PlayerWeaponManager`, `PlayerAnimator`…

- `Assets/Scripts/Enemies`  
  `EnemyBase`, comportamientos de movimiento, jefes y estados de jefe.

- `Assets/Scripts/Weapons`  
  Armas, proyectiles, áreas de daño.

- `Assets/Scripts/Upgrades`  
  Mejoras, stats, gestor de upgrades.

- `Assets/Scripts/Game`  
  `GameManager`, `WaveManager`, `LevelManager`, `ObjectPoolManager`, `GameEvents`…

- `Assets/Scripts/UI`  
  `UIManager`, HUD, menús y pantallas.

- `Assets/ScriptableObjects` *(si se usan)*  
  Datos de armas, oleadas, jefes, upgrades, etc.

---

## ⚙️ Requisitos

- **Unity**: versión 2021.x o superior (cambiar por la versión real que hayas usado).
- **.NET / C#**: versión por defecto de esa versión de Unity.
- Sistema de input:  
  - Se puede usar el **Input System** nuevo, pero también es fácil adaptarlo al clásico.

---

## 🚀 Cómo ejecutar

1. Clonar el repositorio:

   ```bash
   git clone https://github.com/<tu-usuario>/<tu-repo>.git
