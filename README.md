# AddBlock
using UnityEngine;
using System.Collections.Generic;
using System.Collections;
using System;
using System.IO;
using System.Xml.Serialization;
// tem q fechar os arqivos depois criar metodo pra serializar desserializar essa bagaça
public class AddBlock : MonoBehaviour
{
	Stream stream = new FileStream("MyFile.txt", FileMode.Create,FileAccess.ReadWrite, FileShare.None);
	StreamWriter wr = new StreamWriter(@"c:\Myfile.txt", true);
	public static List<UnityEngine.Object> lista = new List<UnityEngine.Object> ();
	GameObject cubo; // cubo a ser adicionado
	private Ray ray; // ray
	private RaycastHit hit; // rayHit
	public int distMaxPlace; // distancia maxima para se adicionar blocos


	// Use this for initialization
	void Start ()
	{

	
	}
	
	// Update is called once per frame
	void Update ()
	
{

		//SelecionaBlocos ();
		switch (SelecionaBlocos ()) 
		{
		case 1:
			cubo =  ((GameObject)Resources.Load("cubo-fisica"));
			Debug.Log("O bloco selecionado eh o bloco normal");
			break;

		case 2:
			cubo = ((GameObject)Resources.Load("Esteira"));
			Debug.Log("O bloco selecionado eh a esteira");
			break;

		case 3:
			cubo = ((GameObject)Resources.Load("Sensor"));
			Debug.Log("O bloco selecionado eh o Sensor");
			break;
		case 4 :
			cubo = ((GameObject)Resources.Load("Default"));
			Debug.Log("Bloco default");
			break;

		}

ray = Camera.main.ScreenPointToRay(Input.mousePosition); // detecta a posiçao do mouse


	if(Input.GetKey(KeyCode.LeftControl))
	{
		if (Input.GetMouseButtonDown (0)) // botao esquerdo
		{
			if (Physics.Raycast (ray,out hit,distMaxPlace)) // verifica a distancia maxima
			{

				if(hit.collider.name.Contains ("Esteira") && cubo.name == "Esteira")		// nao deixa colocar esteira encima de esteira
					{
						return;
					}
				if(hit.collider.name == ("GroundCube(Clone)")||hit.collider.name.Contains ("Esteira") || hit.collider.name.Contains("Default"))
					{
					var obj = Instantiate(cubo,new Vector3(hit.collider.transform.position.x, hit.collider.transform.position.y + 1 ,hit.collider.transform.position.z),Quaternion.identity); // adiciona um bloco na posiçao do mouse e 1 unidade a mais no eixo "y"
							lista.Add(obj);
						if(cubo.name.Contains("fisica"))
							wr.WriteLine("cubo-fisica: posX {0}, posY {1}, posZ {2}",cubo.transform.position.x, cubo.transform.position.y, cubo.transform.position.z);

						if(cubo.name.Contains("Esteira"))
							wr.WriteLine("Esteira: posX {0}, posY {1}, posZ {2}",cubo.transform.position.x, cubo.transform.position.y, cubo.transform.position.z);

						if(cubo.name.Contains("Sensor"))
							wr.WriteLine("Sensor: posX {0}, posY {1}, posZ {2}",cubo.transform.position.x, cubo.transform.position.y, cubo.transform.position.z);

						if(cubo.name.Contains("Default"))
							wr.WriteLine("Default: posX {0}, posY {1}, posZ {2}",cubo.transform.position.x, cubo.transform.position.y, cubo.transform.position.z);


					}
					if(hit.collider.name.Contains("Default"))
					{
						var obj = Instantiate(cubo,new Vector3(hit.collider.transform.position.x, hit.collider.transform.position.y + 1 ,hit.collider.transform.position.z),Quaternion.identity);
							lista.Add(obj);
						if(cubo.name.Contains("fisica"))
							wr.WriteLine("cubo-fisica: posX {0}, posY {1}, posZ {2}",cubo.transform.position.x, cubo.transform.position.y, cubo.transform.position.z);
						
						if(cubo.name.Contains("Esteira"))
							wr.WriteLine("Esteira: posX {0}, posY {1}, posZ {2}",cubo.transform.position.x, cubo.transform.position.y, cubo.transform.position.z);
						
						if(cubo.name.Contains("Sensor"))
							wr.WriteLine("Sensor: posX {0}, posY {1}, posZ {2}",cubo.transform.position.x, cubo.transform.position.y, cubo.transform.position.z);
						
						if(cubo.name.Contains("Default"))
							wr.WriteLine("Default: posX {0}, posY {1}, posZ {2}",cubo.transform.position.x, cubo.transform.position.y, cubo.transform.position.z);

					}


			}

		}
	}
	

	//	if (Physics.Raycast (ray, out hit, distMaxPlace))
	//		Debug.Log(hit.collider.transform.position); // mostra a posiçao do cursor 


}

	public int SelecionaBlocos(){
		if (Input.GetKey (KeyCode.Alpha1)) 
		return 1;

		if(Input.GetKey(KeyCode.Alpha2))
		return 2;

		if (Input.GetKey (KeyCode.Alpha3))
			return 3;

		if (Input.GetKey (KeyCode.Alpha4))
			return 4;

		return 0;

	}
}
