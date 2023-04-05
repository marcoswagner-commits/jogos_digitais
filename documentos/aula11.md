## Aula 09 - Criação de Jogos Digitais

> Aula 28/04/2022 
> Atividades da aula - Movimento

## Engine Gráfica Unity3D

- [Conteúdo do Curso - Material sugerido](https://docs.unity3d.com/Manual/Materials.html)


### Passo 3: Movimentos
- [x] Componentes
 - Movimento (Transformação Geométrica - Position)
 - Movimento por meio da "Física"
  - Analisando o componente Rigidbody (massa - arrasto - gravidade - colisão)
- [x] Código (Script)
 - Criando um Rigidbody no código (público e privado)
  - Associar o Rigidbody pela Unity
  - Associar o Rigidbody pelo método Start (GetComponent<Rigidbody>)
  - FixedUpdate - adicionando força (x,y,z)  
  - Controlando a velocidade (inputs)
   - InputManager (GetAxis)
  - Editando inputs(Project Settings - Inputs)

🎬
[![material complementar](https://github.com/marcoswagner-commits/projetos_cg/blob/aa3f6a6ace359cfac3b5b9f9758fb9c642fe950b/Capa_Aula_Unity3D.png)](https://www.youtube.com/watch?v=i_1jef-1pgQ)
 
#### Script
 ```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Jogador : MonoBehaviour
{
    Rigidbody rg;

    public float velocidade;
     
    // Start is called before the first frame update
    void Start()
    {
       rg = GetComponent<Rigidbody>();
    }
    // Update is called once per frame
    void Update()
    {
       
    }

    private void FixedUpdate() 
    {
      float horizontal = Input.GetAxis("Horizontal");
      float vertical = Input.GetAxis("Vertical");
      Vector3 movimento =  new Vector3(horizontal,0,vertical);
      rg.AddForce( movimento * velocidade);
    }
}
 ```
 
 ### Outra abordagem para movimentos
 
```
public class Jogador : MonoBehaviour
{

    public CharacterController controller;
    public Animator animator;

    public float speed = 1;
    public float gravity = 20;

    public float rotSpeed = 200;

    private float rot;

    private Vector3 dir;
    // Start is called before the first frame update
    void Start()
    {
        // controller = GetComponent<CharacterController>();
        // animator = GetComponent<Animator>();
    }

    // Update is called once per frame
    void Update()
    {
        Move();
    }

    void Move() 
    {
      
      if (controller.isGrounded) 
      {
        if (Input.GetKey(KeyCode.X)) 
        {
          dir = Vector3.forward * speed;
          animator.SetBool("walk",true);

        }
        if (Input.GetKeyUp(KeyCode.X)) 
        {
          dir = Vector3.zero;
          animator.SetBool("walk",false);
        }
      }
        dir.y -= gravity * Time.deltaTime;

        rot += Input.GetAxis("Horizontal") * rotSpeed * Time.deltaTime;
        transform.eulerAngles = new Vector3(0,rot,0);

        dir = transform.TransformDirection(dir);

        controller.Move(dir * Time.deltaTime);
      
    }
}
```
 
