# 292-Kimchi-and-Sushi

---

# **Player Character Script for Godot 4**
This project contains a **2D player character** with movement, jumping, attacks, and hit detection. The script is written in **GDScript** for **Godot 4**.

## **Features**
- **Smooth Movement**: Walk, run, and jump.
- **Attack System**: Weak, strong, and special attacks with hit detection.
- **Damage System**: The player can take damage and has invincibility after being hit.
- **Gravity & Physics**: The player moves naturally with gravity.

---

## **Code Explanation**
### **1. Player Movement (`player.gd`)**
This script controls the player’s movement and animations.

#### **Player States**
The player can be in different states:
```gdscript
enum PlayerState {
	IDLE, WALK, RUN, JUMP, FALL, LAND,
	WEAK_ATTACK, STRONG_ATTACK, SPECIAL_ATTACK,
	HURT, DEATH
}
```
- **IDLE**: Standing still.
- **WALK / RUN**: Moving left or right.
- **JUMP / FALL / LAND**: Jumping, falling, and landing.
- **ATTACKS**: Weak, strong, and special attacks.
- **HURT / DEATH**: When taking damage or dying.

#### **Movement Logic**
- The player moves left/right with `A/D` or arrow keys.
- Holding `Shift` makes the player **run** instead of walk.
- Pressing `Space` makes the player **jump** if they are on the ground.
- Gravity is applied to make the player fall naturally.

```gdscript
if Input.is_action_pressed("ui_right"):
    velocity.x = WALK_SPEED
```

---

### **2. Attack System (`attack_area.gd`)**
The attack system has **three attack types**:
- **Weak Attack (`Z`)**: Small hitbox, fast attack.
- **Strong Attack (`X`)**: Medium hitbox, stronger attack.
- **Special Attack (`C`)**: Large hitbox, most powerful attack.

#### **How It Works**
1. The **attack area (`AttackArea`)** is normally **disabled**.
2. When attacking, the correct **collision shape** is enabled.
3. If an enemy enters the area, it takes damage.
4. After the animation ends, the attack area is **disabled** again.

```gdscript
func enable_attack(attack_type: int) -> void:
    current_attack_type = attack_type
    match attack_type:
        AttackType.WEAK:
            collision_shape_weak.disabled = false
        AttackType.STRONG:
            collision_shape_strong.disabled = false
        AttackType.SPECIAL:
            collision_shape_special.disabled = false
```

---

### **3. Damage System**
- The player has **100 HP**.
- When hit, they enter the `HURT` state.
- After being hit, they are **invincible for 1 second**.
- If HP reaches **0**, the player enters the `DEATH` state.

```gdscript
func take_damage(amount: int) -> void:
    if invincible or state == PlayerState.DEATH:
        return
    hp -= amount
    if hp <= 0:
        die()
    else:
        state = PlayerState.HURT
        invincible = true
        invincible_timer = 1.0
```

---

## **How to Use**
1. **Set Up Player Controls in Godot**
   - `ui_left` → Move Left (`A` or Left Arrow)
   - `ui_right` → Move Right (`D` or Right Arrow)
   - `ui_jump` → Jump (`Space`)
   - `Z` → Weak Attack
   - `X` → Strong Attack
   - `C` → Special Attack

2. **Add an Enemy**
   - Make an enemy node with a `take_damage(amount)` function.

3. **Run the Scene**
   - Press `Play` to test the movement and attack system.

---
