# 최적화 알고리즘을 이용한 회귀식 추정

`202101614 오수민`

## 회귀식 추정

`회귀 분석`

-관찰된 연속형 변수들에 대해 두 변수 사이의 모형을 구한뒤 적합도를 측정해 내는 분석 방법

![image](https://www.tibco.com/sites/tibco/files/media_entity/2020-09/regression-analysis-diagram.svg)

-회귀분석에서는 원인이 되는 변수와 결과로 나타나는 변수를 구분한다. 결과에 영향을 미칠 것으로 예상되는 변수를 독립변수, 설명변수 또는 예측변수라 한다. 반면에, 독립변수의 영향을 받는 변수를 종속변수 또는 반응변수라 한다.

-간단하게 '키(Height)에 따른 몸무게(Weight)' 를 예로 들면, `Weight = a + b*Height` 가 되며, 결국 Height에 따라 Weight가 결정되기 때문에 Height는 독립변수, Weight는 종속변수에 해당한다.

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile2.uf.tistory.com%2Fimage%2F99E4663F5B7918E612CE3A)

---

## 단순회귀분석(simple regression analysis)
하나의 독립변수로 하나의 종속변수를 설명하는 모형이다. 예를 들면, 아버지의 키로 한 자녀의 키를 설명하는 경우에 해당한다.

`단순회귀분석`이란, 연속형 독립변수가 연속형 종속변수에 미치는 영향을 검증하는 분석 방법입니다. 독립변수의 변화에 의해 종속변수가 어떻게 변화하는지를 검증하는 분석 방법이다.

```
#include <stdio.h>
#include <stdlib.h>

float dis(float x, float y, float a, float b) {
	return abs(a * x - y + b) / sqrt(a * a + 1);
}

float dfabda(float a, float b, float da) {
	return (f(a + da, b) - f(a, b)) / da;
}
float dfabdb(float a, float b, float db) {
	return (f(a, b + db) - f(a, b)) / db;
}

int main() {
	ifstream out("simple.txt");
	out >> N;
	cout << N << endl;
	datax = new float[N];
	datay = new float[N];
	for (int i = 0; i < N; i++) {
		out >> datax[i] >> datay[i];
		cout << datax[i] << "   " << datay[i] << endl;
	}
	float a0 = 0, b0 = 0;
	int iteration = 0;
	float eta = 0.0001;
	float psi = 0.005;
	float da = 0.01;
	float db = 0.01;
	float a1 = 3, b1 = 10;
	while (EE(a0, b0, a1, b1) > eta && iteration < 1000000) {
		a0 = a1;
		b0 = b1;
		a1 -=   psi * dfabda(a0, b0, da);
		b1 -=   psi * dfabdb(a0, b0, db);
		iteration++;
	}
	cout << " y  = " << a1 << "x + " << b1 << endl;
	cout << iteration << "-th  E = " << EE(a0, b0, a1, b1) << endl;
	return 0;
}
```

## 유전 알고리즘

`유전 알고리즘`

-유전자 알고리즘은 다윈의 진화론으로부터 창안된 해 탐색 알고리즘이다.

-적자생존의 개념을 최적화 문제를 해결하는 데에 적용한 것이다.

-유전자 알고리즘은 여러 개의 해를 임의로 생성하여 이들을 초기 세대인 G0로 놓은 후, repeat-루프에서 현재 세대의 해로부터 다음 세대의 해를 생성한다. 그리고 루프가 끝났을 때 마지막 세대에서 가장 우수한 해를 리턴한다. 이 해들은 repeat-루프의 반복적인 수행을 통해서 최적해 또는 최적해에 근접한 해가 될 수 있으므로 후보해라고도 일컫는다.

-이 알고리즘에서 가장 핵심적인 부분은 3가지의 연산을 통해 다음 세대의 후보해를 생성한다는 점이다.

`선택 연산`
`교차 연산`
`돌연변이 연산`

```
1. 선택 연산

-`선택 연산`이란, 현재 세대의 후보해 중에서 우수한 후보해들을 선택하는 연산이다.

-현재 세대에 n개의 후보해가 있으면 이들 중에서 우수한 후보해는 중복되어 선택될 수 있고, 적합도가 상대적으로 낮은 후보해들은 선택되지 않을 수도 있다. 이렇게 선택된 후보해의 수는 n개로 유지되는데 이러한 선택은 적자생존 개념을 모방한 것이라고 할 수 있다.

-선택연산은 간단히 룰렛 휠 방법을 사용하여 구현할 수 있다. 이 방법은 각 후보해의 적합도에 비례하여 원반의 면적을 할당하고, 원반을 회전시켜서 원반이 멈추었을 때 핀이 가리키는 후보해를 택한다. 따라서 면적이 넓은 후보해가 선택될 확률이 높다.

2. 교차 연산

-`교차 연산`이란, 선택 연산 후 후보해 사이에 수행되는 것으로 염색체가 교차하는 것을 그대로 모방한 연산이다.

-이 연산의 목적은 선택 연산을 통해 얻은 해보다 더 우수한 후보해를 생성해내는 것이다.

3. 돌연변이 연산

-`돌연변이 연산`이란, 교차 연산이 수행된 후 행해지는 것으로 아주 작은 확률로 후보해의 일부분을 임의로 변형시키는 연산이다.

-돌연변이 연산이 수행된 후의 후보해의 적합도가 전보다 더 나빠질 수도 있다.

-이러한 연산의 목적은 다음 세대에 돌연변이가 이루어진 후보해와 다른 후보해를 교차 연산함으로써 이후 세대에서 보다 우수한 후보해를 생성하기 위함이다.
```

위와 같은 과정이 반복되는 유전 알고리즘은 더 이상 우수한 해가 출현하지 않을 때 알고리즘을 종료시킨다.

```
public class GeneticAlogorithm
{
	public double solve (int nCandidates, Problem p) {
	    int nGenerations = 1000;
	    double[] candidates = new double[nCandidates];
	    //initialize candidates
	    for (int i = 0; i<nGenerations; i++){
	        cnadidates = select(candidates, p);
	        candidates = crossover(candidates, p);
	        candidates = mutate(candidates);
	    }
	    return 0;
	}
	private double[] select(double[] candidates, Problem p) {
	    // Roulette Wheel - Cumulative probability
	    
	    int nCandidates = candidates.length;
	    double[] fitness = new double[nCandidates];
	    //double fitness = p.fit(candidates[0])	  
	    double sum = 0;
	    
	    
	    for (int i=0; i<nCandidates; i++){
	        fitness[i] = p.fit(candidates[i]);
	        sum += fitness[i];
	    }
	    double [] selectedCandidates = new double[nCandidates];
	    
	    for (int i = 0; i < nCanddidates; i++){
	            double pr = Math.random(); //[0,1]
	            if (pr < fitness[1]/sum(x)) y <-fitness[0];
	            else if(pr < fitenss[2]/sum(x)) y <-fitness[1];
	            else if(pr < fitenss[2]/sum(x)) y <-fitness[2];
	            ekse t <- sekectedCabdudates[i] = fitness[3];
	    }


	    return selectedCandidates;
	}
		private double[] crossover(double[] candidates, Problem p) {

	    return null;
	}
		private double[] mutate(double[] candidates) {

	    return null;
	}
	
	public class Main{
	    public static void main(String[] args){
	        //Genetic Algorithm ga = new double[nCandidates];
	        //double solution = ga.solve (4, x -> x*x + 38*x + 80);
	        //System.out.println(solution)
	        for (int i = 0; i < nCandidates; i++){
	            fit
	        }
	    }
	}
}
```
