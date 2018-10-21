������������������ꡣ
�кܶ�𰸣�������Ϊ��õĽ������������Youtube��Ƶ�� ��ι���һ���򵥵ĵ�һ�˳ƿ�������

��ֻ�����������������Ƭ�Ρ�
�Ҵ�������һ����  MouseCameraController �����丽�ӵ����ǵ�  ��
�����

����ű�ʹ�����ܹ��ڲ��ƶ���ɫ�������ʹ��������²鿴����������ϣ����ˮƽ�����ϸı����ǵ���ҵ���תֵ��




using UnityEngine;

public class MouseCameraContoller : MonoBehaviour
{
    public float Sensitivity = 5.0f;
    public float Smoothing = 2.0f;

    private Vector2 _mouseLook;
    private Vector2 _smoothV;

    private GameObject _player;

    void Awake()
    {
        _player = transform.parent.gameObject;
    }

    // Update is called once per frame
    void Update () {
        Vector2 mouseDirection = new Vector2(Input.GetAxisRaw("Mouse X"), Input.GetAxisRaw("Mouse Y"));

        mouseDirection.x *= Sensitivity * Smoothing;
        mouseDirection.y *= Sensitivity * Smoothing;

        _smoothV.x = Mathf.Lerp(_smoothV.x, mouseDirection.x, 1f / Smoothing);
        _smoothV.y = Mathf.Lerp(_smoothV.y, mouseDirection.y, 1f / Smoothing);

        _mouseLook += _smoothV;
        _mouseLook.y = Mathf.Clamp(_mouseLook.y, -90, 90);

        transform.localRotation = Quaternion.AngleAxis(-_mouseLook.y, Vector3.right);
        _player.transform.rotation = Quaternion.AngleAxis(_mouseLook.x, _player.transform.up);
    }
}


There��s a lot of math to digest for this code, but let��s go through them.

Like mentioned, it��s important that the script is attached to the camera and not the main character body.

We want to rotate the characters body when we look left to right, but when we look up to down, we don��t want to rotate the body.

In our Start() function, we want to get the parent element (the Player Game Object) so we can rotate it as we turn.
In Update() we create a vector that would represent where our mouse is facing which we will use to determine where our camera will be facing by using our Mouse X and Mouse Y positions
Next we want to multiply the direction we created with 2 variables: Sensitivity and Smoothing
Sensitivity is used to determine how big the vector of our mouseDirection would be. The higher the number, the more sensitive our mouse would be, allowing us to rotate more quickly.
Smoothness is used to help us evenly move our camera from its starting location to the new location from the mouse location. We use lerp to help us make gradual movement instead of one massive movement
Once we calculate the new direction we��re facing into smoothV, we add that into our mouseLook variable which is where our character will be facing.
We make sure to clamp the y rotation, because we don��t want to be able to go from looking in front of us to behind us. It��s unnatural human anatomy.
Finally, for rotation. We want to rotate the localRotation of our camera, meaning we rotate the game object, relative to its parent.
For example, if our player is facing 90 degree, if we were to set rotation of our camera to 90, we would be at 90, the same location. If we use localRotation, it would take in the fact our character is facing 90 degree and rotate 90 more.
We create our angle by using our y value (the up and down) and rotating by the angle Vector3.right, which is the X axis.
Our last rotation is when we want to turn our player around instead of the camera. We don��t have to use localRotation, because the player doesn��t have a parent (which means it defaults to the game world as the parent), so we can just use rotation, though both would work.
As for our rotation, we rotate our player by our left and right mouse movements using Vector3.up, which is the Y axis.
And that��s it! Play the game and you should be able to move your character around in the game world and bump into our environment that we setup.


��δ����кܶ����ѧ֪ʶ��Ҫ��⣬���������������ǡ�
����ǰ���ᵽ�ģ���Ҫ���ǽű��Ǹ��ӵ�����ϵĶ��������Ƕ����ϡ�

���������ҿ���ʱ��������Ҫ��ת��ɫ�����壬���ǵ��������Ͽ���ʱ�����ǲ�����ת���塣

1�������ǵ�Start���������У�����Ҫ��ȡ��Ԫ�أ�Player Game Object�����������ǿ���������ת����ʱ����ת����

2����Update�����У����Ǵ���һ�������������������ǵ�������ڵ�λ�ã����ǽ�ͨ��ʹ�����ǵ�   Mouse X ��   Mouse Y  λ����ȷ�����ǵ���������ٵ�λ�á�

������������Ҫ�������Ǵ�����2��������Sensitivity��Smoothing���ķ���

1. Sensitivity ����ȷ�����ǵ���귽���ʸ���ж������Խ�ߣ����ǵ����Խ���У��������Ǹ������ת��

2. Smoothing ���ڰ������ǽ������ԭʼλ�þ��ȵ��ƶ������λ�õ���λ�á�����ʹ��lerp�������������˶�������һ�δ��ģ���˶�

3��һ�����Ǽ����������ٵ��µķ���  smoothV��������ӵ����ǵ�  mouseLook �������������ǵĽ�ɫ������ġ�

4������ȷ������y��ת����Ϊ���ǲ�ϣ���ܹ���������ǰ��������������ǲ���Ȼ���������ѧ��

5�������ת����������ת���ǵ�����ı�����ת��localRotation��������ζ��������������ĸ��ڵ���ת��Ϸ����

���磬������ǵ�������90�ȣ�������ǽ��������ת����Ϊ90����ô���ǽ���90�ȣ���ͬ��λ�á��������ʹ��localRotation�����ǵĽ�ɫ���������90���ٴ���ת90�ȡ�

����ͨ��ʹ�����ǵ�yֵ�������ǵĽǶȣ���ͨ����ΪX���Vector3.right�Ƕ���ת��

6������������ת�ǵ�������Ҫ��ת���ǵĽ�ɫ����������������ǲ���ʹ��localRotation����Ϊ���û�и��ڵ㣨����ζ����Ĭ��Ϊ��Ϸ������Ϊ���ڵ㣩����������ֻ��ʹ����ת���������߶���������������

�������ǵ���ת������ͨ�����ǵ���������ƶ���ת���ǵĽ�ɫ��Vector3.up��Y�ᡣ

��������������Ϸ����Ӧ���ܹ�����Ϸ�������ƶ���Ľ�ɫ�������������õĻ�������ײ��
