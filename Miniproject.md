# Ex.No: 10  Implementation of 2D/3D game 
### DATE: 25-05-2025
### Name : SURENDAR N
### REGISTER NUMBER : 212222040165
### Game Name : TargetBot- AI Shooter Arena
### AIM: 
To develop a game TargetBot: AI Shooter Arena in Unity 
### Algorithm:
1. Start Scene → Load Player, AI enemies, and environment.
2. Player Input → WASD to move, mouse to aim, click to shoot.
3. Enemy AI Logic:
      Patrol if player is not nearby.
      Chase and shoot if player is within range.
4. Collision Detection → Detect bullets hitting enemies or the player.
5. Health Update → Reduce HP on hit. Destroy on 0 HP.
6. Win/Loss Check → All enemies defeated or player dies. 
### Program:

#### Player Movement (PlayerMovement.cs)
```c
using UnityEngine;
public class PlayerMovement : MonoBehaviour
{
    public float speed = 6f;
    public CharacterController controller;
    void Update()
    {
        float moveX = Input.GetAxis("Horizontal");
        float moveZ = Input.GetAxis("Vertical");
        Vector3 move = transform.right * moveX + transform.forward * moveZ;
        controller.Move(move * speed * Time.deltaTime);
    }
}
```
#### Player Shooting (PlayerShoot.cs)
```c
using UnityEngine;
public class PlayerShoot : MonoBehaviour
{
    public GameObject bulletPrefab;
    public Transform firePoint;
    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
        }
    }
}
```
#### Bullet Behavior (Bullet.cs)
```c
using UnityEngine;
public class Bullet : MonoBehaviour
{
    public float speed = 20f;
    public int damage = 10;
    void Update()
    {
        transform.Translate(Vector3.forward * speed * Time.deltaTime);
        Destroy(gameObject, 3f); 
    }
    void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Enemy"))
        {
            other.GetComponent<EnemyHealth>()?.TakeDamage(damage);
            Destroy(gameObject);
        }
        if (other.CompareTag("Player"))
        {
            other.GetComponent<PlayerHealth>()?.TakeDamage(damage);
            Destroy(gameObject);
        }
    }
}
```
#### Enemy AI (EnemyAI.cs)
```c
using UnityEngine;
using UnityEngine.AI;
public class EnemyAI : MonoBehaviour
{
    public Transform player;
    public float detectionRange = 15f;
    public float shootCooldown = 2f;
    public GameObject bulletPrefab;
    public Transform firePoint;
    private NavMeshAgent agent;
    private float shootTimer;
    void Start()
    {
        agent = GetComponent<NavMeshAgent>();
    }
    void Update()
    {
        float distance = Vector3.Distance(transform.position, player.position);
        if (distance < detectionRange)
        {
            agent.SetDestination(player.position);
            transform.LookAt(player);

            shootTimer -= Time.deltaTime;
            if (shootTimer <= 0f)
            {
                Shoot();
                shootTimer = shootCooldown;
            }
        }
    }
    void Shoot()
    {
        Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
    }
}
```
#### Enemy Health (EnemyHealth.cs)
```c
using UnityEngine;
public class EnemyHealth : MonoBehaviour
{
    public int maxHealth = 50;
    private int currentHealth;
    void Start()
    {
        currentHealth = maxHealth;
    }
    public void TakeDamage(int amount)
    {
        currentHealth -= amount;
        if (currentHealth <= 0)
        {
            Destroy(gameObject);
        }
    }
}
```
#### Player Health (PlayerHealth.cs)
```c
using UnityEngine;

public class PlayerHealth : MonoBehaviour
{
    public int maxHealth = 100;
    private int currentHealth;
    void Start()
    {
        currentHealth = maxHealth;
    }
    public void TakeDamage(int amount)
    {
        currentHealth -= amount;
        Debug.Log("Player Health: " + currentHealth);
        if (currentHealth <= 0)
        {
            Die();
        }
    }

    void Die()
    {
        Debug.Log("Player Died!");
    }
}
```
### Output:
![targetbot demo](https://github.com/user-attachments/assets/c19da4ff-cd6a-441a-b981-79a0ae18fd37)

### Result:
Thus the game was developed using Unity and adopted AI technology.
