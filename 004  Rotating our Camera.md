我们如何让相机跟随鼠标。
有很多答案，但我认为最好的解释来自于这个Youtube视频： 如何构建一个简单的第一人称控制器。

我只是拿了相机控制器的片段。
我创建了另一个类  MouseCameraController 并将其附加到我们的  主
相机。

这个脚本使我们能够在不移动角色的情况下使用相机上下查看，但是我们希望在水平方向上改变我们的玩家的旋转值。




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


There’s a lot of math to digest for this code, but let’s go through them.

Like mentioned, it’s important that the script is attached to the camera and not the main character body.

We want to rotate the characters body when we look left to right, but when we look up to down, we don’t want to rotate the body.

In our Start() function, we want to get the parent element (the Player Game Object) so we can rotate it as we turn.
In Update() we create a vector that would represent where our mouse is facing which we will use to determine where our camera will be facing by using our Mouse X and Mouse Y positions
Next we want to multiply the direction we created with 2 variables: Sensitivity and Smoothing
Sensitivity is used to determine how big the vector of our mouseDirection would be. The higher the number, the more sensitive our mouse would be, allowing us to rotate more quickly.
Smoothness is used to help us evenly move our camera from its starting location to the new location from the mouse location. We use lerp to help us make gradual movement instead of one massive movement
Once we calculate the new direction we’re facing into smoothV, we add that into our mouseLook variable which is where our character will be facing.
We make sure to clamp the y rotation, because we don’t want to be able to go from looking in front of us to behind us. It’s unnatural human anatomy.
Finally, for rotation. We want to rotate the localRotation of our camera, meaning we rotate the game object, relative to its parent.
For example, if our player is facing 90 degree, if we were to set rotation of our camera to 90, we would be at 90, the same location. If we use localRotation, it would take in the fact our character is facing 90 degree and rotate 90 more.
We create our angle by using our y value (the up and down) and rotating by the angle Vector3.right, which is the X axis.
Our last rotation is when we want to turn our player around instead of the camera. We don’t have to use localRotation, because the player doesn’t have a parent (which means it defaults to the game world as the parent), so we can just use rotation, though both would work.
As for our rotation, we rotate our player by our left and right mouse movements using Vector3.up, which is the Y axis.
And that’s it! Play the game and you should be able to move your character around in the game world and bump into our environment that we setup.


这段代码有很多的数学知识需要理解，让我们来看看它们。
就像前面提到的，重要的是脚本是附加到相机上的而不是主角对象上。

当我们向右看的时候，我们想要旋转角色的身体，但是当我们向上看的时候，我们不想旋转身体。

1）在我们的Start（）函数中，我们要获取父元素（Player Game Object），所以我们可以在我们转动的时候旋转它。

2）在Update（）中，我们创建一个向量，它将代表我们的鼠标所在的位置，我们将通过使用我们的   Mouse X 和   Mouse Y  位置来确定我们的相机将面临的位置。

接下来，我们要乘以我们创建的2个变量（Sensitivity和Smoothing）的方向。

1. Sensitivity 用于确定我们的鼠标方向的矢量有多大。数字越高，我们的鼠标越敏感，允许我们更快地旋转。

2. Smoothing 用于帮助我们将相机从原始位置均匀地移动到鼠标位置的新位置。我们使用lerp来帮助我们逐步运动而不是一次大规模的运动

3）一旦我们计算我们面临到新的方向  smoothV，我们添加到我们的  mouseLook 变量，它是我们的角色将面向的。

4）我们确保控制y旋转，因为我们不希望能够从我们面前看到我们身后。它是不自然的人体解剖学。

5）最后，旋转。我们想旋转我们的相机的本地旋转（localRotation），这意味着我们相对于它的父节点旋转游戏对象。

例如，如果我们的玩家面对90度，如果我们将相机的旋转设置为90，那么我们将在90度，相同的位置。如果我们使用localRotation，我们的角色将从面向的90度再次旋转90度。

我们通过使用我们的y值创建我们的角度，并通过作为X轴的Vector3.right角度旋转。

6）我们最后的旋转是当我们想要旋转我们的角色而不是摄像机。我们不必使用localRotation，因为玩家没有父节点（这意味着它默认为游戏世界作为父节点），所以我们只能使用旋转，尽管两者都可以正常工作。

对于我们的旋转，我们通过我们的左右鼠标移动旋转我们的角色，Vector3.up是Y轴。

就是这样！玩游戏，你应该能够在游戏世界中移动你的角色，并在我们设置的环境中碰撞。
