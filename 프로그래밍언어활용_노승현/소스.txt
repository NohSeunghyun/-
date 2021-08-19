package producttest;

import java.util.*;

interface ProductInter{
	
	static final int TV = 1, COMPUTER = 2, AUDIO = 3, REFUND = 4;
	
	static void buyEnter() {
		System.out.println("구매자가 전자마트에 입장하셨습니다.");
		
	}
	
	void menu();
}

abstract class Product {
	
	final String makeCountry = "대한민국";
	int price;
	int bonusPoint;
	
	public Product(int price) {
		super();
		this.price = price;
		bonusPoint = (int)(price/10.0);
	}
	
	abstract void show();
}

class TV extends Product {

	final String maker = "삼성";
	
	public TV() {
		super(200);
	}

	@Override
	void show() {
		System.out.println("제조국 : " + makeCountry + ", 제조사 : " + maker + ", 가격 : " + price + "만원, 보너스포인트 : " + bonusPoint + "점 입니다.");
	}

	@Override
	public String toString() {
		return "TV";
	}
	
}
class Computer extends Product {
	
	final String brand = "LG 그램";
	
	public Computer() {
		super(100);
	}
	
	@Override
	void show() {
		System.out.println("제조국 : " + makeCountry + ", 브랜드 : " + brand + ", 가격 : " + price + "만원, 보너스포인트 : " + bonusPoint + "점 입니다.");
	}
	
	@Override
	public String toString() {
		return "Computer";
	}
	
}
class Audio extends Product {
	
	public Audio() {
		super(50);
	}
	
	@Override
	void show() {
		System.out.println("제조국 : " + makeCountry + ", 가격 : " + price + "만원, 보너스포인트 : " + bonusPoint + "점 입니다.");
	}
	
	@Override
	public String toString() {
		return "Audio";
	}
	
}

class Buyer {
	
	int money;
	int bonusPoint = 0;
	int cntTv, cntCom, cntAud;
	
	
	List<Product> item = new ArrayList<Product>();
	
	public Buyer (int money) {
		this.money = money;
	}
	
	void askInfo(Product p) {
		System.out.println(p+"제품에 대한 정보 부탁드립니다.");
		p.show();
	}
	
	void buy(Product p) {
		if (this.money < p.price) {
			System.out.println("잔액이 부족하여 제품을 살 수 없습니다.");
			return;
		}
		this.money -= p.price;
		this.bonusPoint += p.bonusPoint;
		item.add(p);
		
		System.out.println(p+"을(를) 구입하셨습니다.");
	}
	
	void refund(Product p) {
		if (item.remove(p)) {
			this.money += p.price;
			this.bonusPoint -= p.bonusPoint;
			System.out.println(p+"을(를) 반품하셨습니다.");
			} else System.out.println("구입하신 제품은 저희 제품 목록중에 없습니다."); 
	}
	
	void summary() {
		int sum = 0;
		String itemList1 = "";
		
		int i = 0;
		if(!item.isEmpty()) {
			for (Product itemList : item) {
				sum += itemList.price;
				itemList1 = itemList1 + ((i==0) ? "" + item.get(i) : ", "  + item.get(i));
				i++;
			}
			System.out.println("**********구입한 제품목록과 총금액**********");
			System.out.println("구입하신 제품은 [" + itemList1 + "]이고");
			System.out.println("구입하신 제품의 총 금액은" + sum + "만원입니다.");
		}
		else System.out.println("구매자는 제품을 구매하지 않았습니다.");
		}
		
	
}
	
public class ProductTest implements ProductInter {
	
	@Override
	public void menu() {
		System.out.println("*************가전제품 목록*************");
		System.out.println("1.TV   2.Computer   3.Audio   4.환불");
	}
	
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);
		
		Buyer NSH = new Buyer(1000);
		
		TV tv = new TV();
		Computer computer = new Computer();
		Audio audio = new Audio();
		
		ProductInter.buyEnter();
		
		ProductTest pt = new ProductTest();
		
		String tmp = "";
		
		while (NSH.money != 0) {
			System.out.println();
			pt.menu();
			System.out.print("원하시는 가전제품 번호 입력(단, 끝내려면 stop입력) > ");
			tmp = sc.next();
			
			if (tmp.equalsIgnoreCase("stop")) break;
			int num = 0;
			try {
				num = Integer.parseInt(tmp);
			} catch (NumberFormatException e) {
				System.out.print("잘못된 값을 입력하셨습니다.");
				System.out.println(e.getMessage());
				continue;
			}
			switch (num) {
				case ProductInter.TV : 
					NSH.askInfo(tv);
					NSH.buy(tv);
					continue;
				case ProductInter.COMPUTER : 
					NSH.askInfo(computer);
					NSH.buy(computer);
					continue;
				case ProductInter.AUDIO : 
					NSH.askInfo(audio);
					NSH.buy(audio);
					continue;
				case ProductInter.REFUND : 
					System.out.print("환불할 제품 입력(Tv, Computer, Audio) > ");
					tmp = sc.next();
					if (tmp.equalsIgnoreCase("tv")) NSH.refund(tv);
					if (tmp.equalsIgnoreCase("computer")) NSH.refund(computer);
					if (tmp.equalsIgnoreCase("audio")) NSH.refund(audio);
					continue;
			}
		}//while문종료
		System.out.println();
		System.out.println("**********가전제품 판매를 종료합니다.**********");
		NSH.summary();
	}
	
}