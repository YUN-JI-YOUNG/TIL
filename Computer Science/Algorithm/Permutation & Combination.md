## Permutation (순열)      
###### 순열은 순서를 고려하면서 조합을 뽑는 방법이다.        

#### javascript 로 구현    
```javascript     
function getPermu(arr, n) {
  const result = [];
  if (n === 1) return arr.map((el) => [el]);
  arr.forEach((fixed, idx, origin) => {
      const rest = [...origin.slice(0, idx), ...origin.slice(idx+1)];
      const permu = getPermu(rest, n-1);
      const attached = permu.map((el) => [fixed, ...el]);
      result.push(...attached);
  });
  return result;
}
```       
</br>     



## Combination (조합)     
###### 조합은 순서를 고려하지않고 조합을 뽑는 방법이다.     


