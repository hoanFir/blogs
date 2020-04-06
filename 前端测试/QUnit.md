
一个用于构建单元测试的成熟稳定的平台。

示例：

```javascript

functin calculateShipping(distance) {

  switch(distance) {
    case (distance < 25): shipping = 4; 
      break;
    case (distance < 100): shipping = 5;
      break;
    case (distance < 1000): shipping = 6;
      break;
    case (distance >= 1000): shipping = 7;
      break;
  }
  return shipping;

}

QUnit.test('Calculate Shipping', function(assert) {
  asert.equal(calculateShipping(24),4,"24 Miles");
  assert.equal(calculateShipping(99),5,"99 Miles"); 
  assert.equal(calculateShipping(999),6,"999 Miles");
  assert.equal(calculateShipping(1000),7,"1000 Miles");  
})


```

