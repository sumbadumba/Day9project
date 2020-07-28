#include<stdio.h>
#include<string>
#include<iostream>
#include<Windows.h>
#include<math.h>//수학관련
#include<time.h>
#include<stdlib.h>


using namespace std;

struct Character
{
	string name;
	float hp;
	float mana = 100.0f;
	float attack;
	float defence;
	float critRate;
	float critDamage;
	float evasionRate = 0.1f;
	float maxHp;

	void Inti(string name, float hp , float attack,float defence, float critRate,float critDamage,float evasionRate = 0.1f )
	{//이렇게 쓰면 값을 안넣어 줘도 됨.
		this->name = name;
		this->hp = hp;
		this->mana = mana;
		this->attack = attack;
		this->defence = defence;
		this->critRate = critRate;
		this->critDamage = critDamage;
		this->evasionRate = evasionRate;
		maxHp = hp;
	}//("플레이어", 1000.0f, 100.0f, 20.0f, 0.3f, 1.5f);
	bool CheckCrit()//크리터지면 true
	{//숫자가 높아질 수록 회피율이 낮아짐(역수)
		float randRange = rand() % 101 / 100.0f;
		if (critRate > randRange)
			return true;
		else
			return false;

	}
	// 회피뜨면 true 아니면 false
	bool CheckEvassion()
	{//숫자가 높아질 수록 회피율이 낮아짐(역수)
		float randRange = rand() % 101 / 100.0f;
		if (evasionRate > randRange)
		{
			cout << name << "의 회피 !" << endl;
			return true;
		}
		else
			return false;
	}
	bool CheckDead()
	{
		if (hp <= 0.0f)
		{
			cout << name << "이(가) 사망 하였습니다. " << endl;
			return true;
		}
		else
			return false;
	}
	void PrintCurrentHP()
	{
		cout << name << "의 남은 체력은 : " << hp << " / " << maxHp << endl;

	}
	bool CheckHeal()
	{
		if (hp <= maxHp * 0.3f)
		{
			cout << name << "이가 체력을 회복합니다 !!" << endl;
			SilllHeal();
			return true;
		}
		else
			return false;
	}

	void SilllHeal()
	{
		if (mana >= 50)
		{
			mana -= 50;
			hp += maxHp * 0.3f;

			if (hp > maxHp)
			{
				hp = maxHp;
			}

			cout << name << "이(강)" << maxHp * 0.3f << "만큼의 체력을 회복하였습니다. " << endl;
			PrintCurrentHP();
		}
		else
		{
			cout << "마나가 부족합니다. " << endl;
		}
	}


	//Pameter : other -> 피격 대상 
	// 최종 데미지 리턴 
	float CalculateDamage(Character other)
	{
		float randRandge = (((rand() & 21) / 100.0f) - 0.1f);//0.1f ~ 0.1f
		float tempDamage = attack + (attack * randRandge);//f는 어택의 10%정도 된다고 볼 수 있다.
		if (CheckCrit())
		{
			tempDamage *= critDamage;
		}
		return roundf(tempDamage - other.defence);//0부터 4까지는 버리고 5부터는 쓴다.
	}


};

bool BattleLogic(Character * attacker, Character * defencer)
{
	if (attacker->CheckDead())
	{
		return false;
	}
	if (attacker->CheckHeal() == false
		&& defencer->CheckEvassion() == false)
	{
		cout << attacker->name << "의 공격 !!" << endl;
		float damage = attacker->CalculateDamage(*defencer);
		defencer-> hp -= damage;
		cout << defencer->name << "은(는) " << damage << "만큼의 데미지를 입었다. " << endl;
		defencer->PrintCurrentHP();
	}
	return true;
}

int main()
{//한글을 쓸때 wstring을 쓴다.(그래야 잘 처리됨)
	//srand(time(NULL));
	srand((unsigned int)time(NULL));

	Character player;
	player.Inti("플레이어", 1000.0f, 100.0f, 20.0f, 0.3f, 1.5f);

	Character monster;
	monster.Inti("몬스터", 500.0f, 70.0f, 10.0f, 0.3f, 1.2f);

	while (true)
	{
		Sleep(2000);//2초
		system("cls");

		if(BattleLogic(&player,&monster) == false)
		{
			break;//플레이어가 죽은 경우
		}
		Sleep(2000);//2초
		system("cls");

		if(BattleLogic(&monster,&player) == false)
			break;//몬스터가 죽은 경우
	}

	return 0;
}

//숙제
// 체스판 그리기
// 1.문자표에 있는 기호들을 이용해서 체스판을 그리기
// 2. 윈도우 검색창에 '문자표'라고 검색하면 문자표 창이 뜬다.(Haan Wing2)
// 3. □ ■ ( 8*8)
// 4. Font는 Arial을 선택



//은(는) 이(가) 처리법은 내일
