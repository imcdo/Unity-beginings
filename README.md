# Unity-beginings
First trys :D
um test code:
using UnityEngine;
using System.Collections;
  [RequireComponent (typeof(CharacterController))]
public class FirstPersonController : MonoBehaviour {

	public float movementSpeed = 10.0f;
	public float rotSpeed = 2.0f;
	public float upDownRange = 60.0f;
	public float jumpSpeed = 5;
	float verticalRotation = 0;
	float recspeed = 0;
	float recsidespeed = 0;
	float forwardSpeed = 0;
	float sideSpeed=0;
	float rotLeftRight=0;
	CharacterController cc;

	float verticalVelocity = 0;


	// Use this for initialization
	void Start () {
	
		Screen.lockCursor = true;
		 cc = GetComponent<CharacterController> ();

	}
	
	// Update is called once per frame
	void Update () {

		//Rotation

		rotLeftRight = Input.GetAxis ("Mouse X")*rotSpeed;
		if (cc.isGrounded) {
						
						transform.Rotate (0, rotLeftRight, 0);
						
				} 
		else {

			//Camera.main.transform.Rotate  (0,rotLeftRight,0);
			Camera.main.transform.localRotation = Quaternion.Euler(0,rotLeftRight,0);
		}
		verticalRotation -= Input.GetAxis ("Mouse Y") * rotSpeed;

		verticalRotation = Mathf.Clamp (verticalRotation, -upDownRange, upDownRange);
		Camera.main.transform.localRotation = Quaternion.Euler(verticalRotation,0,0);

		//Movement
		if (cc.isGrounded) {
						forwardSpeed = Input.GetAxis ("Vertical") * movementSpeed;
						sideSpeed = Input.GetAxis ("Horizontal") * movementSpeed;
				}


		verticalVelocity += Physics.gravity.y * Time.deltaTime;
		if (cc.isGrounded){
			recspeed = forwardSpeed;
			recsidespeed = sideSpeed;
		}
		if (Input.GetButtonDown ("Jump") && cc.isGrounded) {

			verticalVelocity = jumpSpeed;
			sideSpeed = recsidespeed;
			forwardSpeed = recspeed;
				}
		Vector3 speed = new Vector3(sideSpeed,verticalVelocity,forwardSpeed);	 
		speed = transform.rotation * speed;



		cc.Move(speed * Time.deltaTime);

	}
}
