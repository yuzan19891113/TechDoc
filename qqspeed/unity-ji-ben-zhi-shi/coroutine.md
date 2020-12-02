# Coroutine

协程：即允许你将执行过程分摊到每一帧去执行

开启协程：startcoroutine

关闭协程：stopcoroutine

协程例子

```text
void Update()
{
    if(Input.GetMouseButtonDown(0))
    {
        StartCoroutine(MoveTank());
    }
}

IEnumerator MoveTank()
    {
        while(facingWrongWay)
        {
            TurnTank();
            yield return null;
        }

        while (notInPosition)
        {
            MoveTank();
            yield return null;
        }

        yield return new WaitForSeconds(1);
        Fire();    
    }
```



