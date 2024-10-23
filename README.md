# 2024-10-23

#include "HX711.h"
 
const int LOADCELL_DOUT_PIN = 3; 
const int LOADCELL_SCK_PIN = 2;  

HX711 scale;

void setup() {
  Serial.begin(9600);
  scale.begin(LOADCELL_DOUT_PIN, LOADCELL_SCK_PIN);
  scale.set_scale(2280.f); // 보정 값 (조정 필요)
  scale.tare(); // 초기 무게 조정
}

void loop() {
  if (scale.is_ready()) { // HX711이 준비되었는지 확인
    float weight = scale.get_units(10); // 평균 무게 계산

    // 무게가 0보다 클 경우만 출력
    if (weight > 0) {
      Serial.print("물체의 무게: ");
      Serial.print(weight, 1); // 소수점 한 자리까지 표시
      Serial.println(" g");
    } else {
      Serial.println("물체가 없음");
    }
  } else {
    Serial.println("HX711이 준비되지 않았습니다.");
  }

  delay(500); // 0.5초 대기
}
