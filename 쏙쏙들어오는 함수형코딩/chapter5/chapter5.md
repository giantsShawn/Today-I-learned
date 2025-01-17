# 더 좋은 액션 만들기
- 암묵적 입력과 출력을 제거해서 재사용성 하기 좋은 코드 만들기
- 복잡하게 엉킨 코드를 풀어 더 좋은 구조로 만드는법을 배움



## 비즈니스 요구사항과 설계를 맞춰라 !
- 요구 사항 : 장바구니에 담긴 제품을 주문할떄 무료 배송인지 확인해라
~~~ js

// 잘못된 예시
// total과 item_price로 무료배송인지 확인
function gets_free_shipping(total,item_price){
    return item_price+total>=20;
}
// 올바른 예시로 전환
function gets_free_shipping(cart){
    return calc_total(cart)>=20;
}
function calc_total(cart){
    var total=0;
    for(var i=0;i<cart.length;i++){
        var item=cart[i]
        total+=item.price;
    }
    return total
}
~~~

## 암묵적 입력과 출력을 적게 할수록 좋다.
- 인자가 아닌 모든 입력은 암묵적 입력이다
- 리턴값이 아닌 모든 출력은 암묵적 출력이다.
> 이 두가지가 존재하면 다른곳에서 사용할수 없기 때문에 모듈이 아니다.
> 계산은 암묵적입력과 출력이 없다 고로 테스트하기가 쉬워진다.



- 아래 코드를 암묵적 입력에서 명시적 입력으로 바꾸기
~~~ js
function add_item_to_cart(name,price){
    shopping_cart=add_item(shoppingcart,name,price)
    calc_cart_total()
}

function calc_cart_total(){
    shopping_cart_total=calc_total(shopping_cart)
    set_cart_total_dom()
    update_shipping_icons(shopping_cart)
    update_tax_dom()
}

function set_cart_total_dom(){
    shopping_cart_total
}

function update_shipping_icons(cart){
    var buy_buttons=get_buy_buttons_dom()
    for(var i=0;i<buy_buttons.length;i++){
        var button=buy_buttons[i]
        var item=button.item
        var new_cart=add_item(cart,item.name,item.price)
        if(get_free_shipping(new_cart))
            button.show_free_shipping_icons()
        else
            button.hide_free_shipping_icons()
    }
}
function update_tax_dom(){
    set_tax_dom(calc_tax(shopping_cart_total))
}
~~~
- 명시적 입력, 출력으로 변환
~~~js

function add_item_to_cart(name,price){
    shopping_cart=add_item(shopping_cart,name,price)
    calc_cart_total(shopping_cart)
}
function calc_cart_total(cart){
    var total=calc_total(cart)
    set_cart_total_dom(total)
    update_shipping_icons(cart)
    update_tax_dom(total)
}

function set_cart_total_dom(total){
    total
}

function update_shipping_icons(cart){
    var buy_buttons=get_buy_buttons_dom()
    for(var i=0;i<buy_buttons.length;i++){
        var button=buy_buttons[i]
        var item=button.item
        var new_cart=add_item(cart,item.name,item.price)
        if(get_free_shipping(new_cart))
            button.show_free_shipping_icons()
        else
            button.hide_free_shipping_icons()
    }
}
function update_tax_dom(total){
    set_tax_dom(calc_tax(total))
}
~~~


2. 설계는 엉켜있는 코드를 푸는것이다
- 함수가 작으면 작을수록 재사용 성이 쉽다
- 작은함수는 이해하기 쉽고 유지보수하기가 쉽다 
- 작은함수는 테스트하기가 쉽다.

~~~ js
function add_item(cart,item){
    var new_cart=cart.slice()
    new_cart.push(item)
    return new_cart
}
~~~
1. 카트에 아이템을 추가하는 add_item 함수를 아래와같이 작게 쪼개볼수있다!

~~~js
function add_element_last(array,elem){
    var new_array=[...array]
    new_array.push(elem)
    return new_array
}
function add_item(cart,item){
    return add_element_last(cart,item)
}
function make_cart_item(name,price){
    return {
        name:name,
        price:price
    }
}
function calc_total(cart){
    var total=0
    for(var i=0;i<cart.length;i++){
        var item=cart[i]
        total+=item.price
    }
    return total
}
function gets_free_shipping(cart){
    return calc_total(cart)>=20
}
function calc_tax(amount){
    return amount*0.10
}

~~~

> 결론
1. 암묵적 입력은 인자로 , 암묵적 출력은 리턴값으로 바꿔라
2. 하나의 함수에서 하나의 일만 하도록하면 유지보수 및 재사용성 측면에서 좋다

