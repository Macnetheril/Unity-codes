using UnityEngine;

public class PlayerController : MonoBehaviour {

    public float Speed = 3f;

    private Vector3 _movement;
    private Rigidbody _playerRigidBody;

    private void Awake()
    {
        _playerRigidBody = GetComponent&lt;Rigidbody&gt;();
    }
    

    private void FixedUpdate()
    {
        float horizontal = Input.GetAxisRaw("Horizontal");
        float vertical = Input.GetAxisRaw("Vertical");

        Move(horizontal, vertical);
    }

    private void Move(float horizontal, float vertical)
    {
        _movement = (vertical * transform.forward) + (horizontal * transform.right);
        _movement = _movement.normalized * Speed * Time.deltaTime;
        _playerRigidBody.MovePosition(transform.position + _movement);
    }
}


The code is pretty straight forward, here’s the flow:

In Awake() we get our RigidBody so we can move our player
In FixedUpdate(), we use GetAxisRaw() to get our horizontal and vertical movement so that we can detect if the user is moving left/right and up/down.
In Move() we’re given the movement directions. We want to create a movement vector that captures the direction from where the user is facing, which we achieve with transform.forward and transform.right. By multiplying everything together, we get the direction that the player should move based off of their inputs.
Finally we normalize our movement vector and then we move our position with our new Vector.
It’s important to note that we must use the RigidBody to move.

Before I was trying to move the player with transform.Translate, what I found out is that this function is the equivalent of teleportation. What happens is that if there are obstacles in front of the player, our character will just teleport past them.

I also want to emphasis that it’s important that we use transform.forward and transform.right especially once we start trying to get the camera to follow the mouse.

When we start rotating our character to follow the mouse, our vertical and horizontal values don’t give us the direction adjusted for which direction we’re facing.

For example, if we were to face forward and press W to move forward, we would move forward. However, if we were to turn 90 degrees to the right, and press W again, instead of moving forward like you’d expect, you would move to the left where “forward” use to be.

transform.forward gives us vector that the player is facing and then we multiply that with our vertical value which tells us if we’re moving forward or back. We do the same with transform.right and horizontal.


代码很简单，流程如下：

1）在Awake ()函数中我们得到我们的刚体，这样我们就可以移动我们的角色了。

2）在FixedUpdate()函数中，我们使用GetAxisRaw ()来得到我们的水平和垂直移动，这样我们就可以检测用户是否在向左/右移动和向上/向下移动。

3）在Move ()函数中我们得到运动方向。我们想要创建一个移动矢量，它捕捉用户所面对的方向，我们通过transform.forward和transform.right实现。通过将所有的东西相乘，我们就得到了玩家应该根据输入而移动的方向。

4）最后，我们使运动向量标准化，然后我们用新的向量来移动我们的位置。

需要注意的是，我们必须使用刚体来移动。

在我试图通过transform.Translate移动角色之前。我发现这个函数相当于传送。如果在玩家面前有障碍，我们的角色就会从他们旁边绕过。
我也想强调，我们使用  transform.forward  和  transform.right很重要，  特别是一旦我们开始尝试让相机跟随鼠标。

当我们开始旋转我们的角色来跟随鼠标时，我们的垂直和水平的值并没有给我们所面对的方向进行调整。

例如，如果我们按W，我们就会前进。然而，如果我们向右旋转90度，再次按W，这是并像你期望的那样向前移动，你就向左移动到本应该“前进”的位置。
transform.forward  给出了玩家面临的向量，然后我们乘以我们的  垂直  值，告诉我们我们是向前还是向后。我们对transform.right 和  horizontal做同样的事情  。
