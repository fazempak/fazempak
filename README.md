- 👋 Hi, I’m @fazempak
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
fazempak/fazempak is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
using System.Collections;
using System.Collections.Generic;
using UnityEngine;// позволяет настраивать взаимодействие объектов
using UnityEngine.SceneManagement; // позволяет работать со сценами


public class PlayerController : MonoBehaviour
{
    [SerializeField] private float _speed = 3f;
    [SerializeField] private float _jumpForce = 3f;
    private float size = 1f; // float - дробное число
    private bool isJump = true; //  bool - логический тип, true - истина, false - ложь
    private Rigidbody2D _rigidbody;

   
    private void Start()
    {
        _rigidbody = GetComponent<Rigidbody2D>();

    }

    
    private void Update()
    {
        

        if (Input.GetKeyDown(KeyCode.Space)&& isJump)
        {
            _rigidbody.velocity = (Vector2.up * _jumpForce);
            isJump = false;

        }

        _rigidbody.velocity = new Vector2(Input.GetAxis("Horizontal") * _speed, _rigidbody.velocity.y);
        

        transform.localScale = new Vector3(size, size, 1f);

        if (size <= 0.9f)
        {

            SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
        }

            Camera.main.transform.position = new Vector3(
             transform.position.x,
             transform.position.y,
              Camera.main.transform.position.z
                );
        

    }
    private void FixedUpdate()
    {
        if (transform.position.x > Camera.main.transform.position.x)

        {
            Camera.main.transform.position = new Vector3(
           transform.position.x,
           Camera.main.transform.position.y,
           Camera.main.transform.position.z
                );
        }
    }
    private void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.gameObject.CompareTag("Apple"))
        {
            Destroy(collision.gameObject);
            size += 0.1f; // += - увеличивает значение переменной

        }
        if (collision.gameObject.CompareTag("spike"))
        {
            //Destroy(collision.gameObject);
            size -= 0.5f; // += - уменьшает значение переменной
            _rigidbody.velocity = new Vector2 (_rigidbody.velocity.x, _jumpForce / 2);

        }
    }
    private void OnCollisionEnter2D(Collision2D collision)// проверка физического столконовения
    {
        if (collision.gameObject.CompareTag("Ground"))// collision.gameobject - объект с которым столкнулся персонаж
        {
            isJump = true;
        }
    }
}
