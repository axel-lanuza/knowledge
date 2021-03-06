## Text
Given a linked list S check if S is a palindrome.

## Solution
```javascript
function isLinkedListNode(node) {
  return !!node && node.hasOwnProperty('value') && node.hasOwnProperty('next');
}

function is_palindrome(start, end) {
  if(!end) {
    return {palindrome: true, start: start};
  }
  let object = is_palindrome(start, end.next);
  return {palindrome: object.palindrome && object.start.value === end.value, start: object.start.next};
}

function palindromeLinkedList(start) {
  if(!isLinkedListNode(start)) {
    return false;
  }
  
  let object = is_palindrome(start, start.next);
  return object.palindrome;
}
```

## Testcase
- palindromeLinkedList({value: 1, next: null}) = true OK
- palindromeLinkedList(null) = false OK
- palindromeLinkedList(undefined) = false OK
- palindromeLinkedList({}) = false OK
- palindromeLinkedList({value: 1, next: {value: 2, next: null}}) = false
- palindromeLinkedList({value: 1, next: {value: 1, next: null}}) = true
- palindromeLinkedList({value: 1, next: {value: 2, next: {value: 1, next: null}}}) = true

## Complexity
O(n)
