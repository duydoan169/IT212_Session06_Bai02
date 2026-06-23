Tầm Quan Trọng Của Thứ Tự Ưu Tiên Tính Toán
Trong nghiệp vụ kế toán và tài chính, thứ tự thực hiện các phép tính không phải là chi tiết kỹ thuật

mà là yêu cầu nghiệp vụ có tính pháp lý. Có 3 lý do cốt lõi:

Sai thứ tự dẫn đến sai số tiền thực tế: Nếu tính VAT trước rồi mới trừ coupon,

khách hàng bị tính thuế trên số tiền chưa được giảm giá - đây là sai về nghĩa vụ thuế.

Ngược lại nếu tính phí vận chuyển trước khi kiểm tra ngưỡng miễn phí,

hệ thống có thể thu phí ship trên những đơn lẽ ra được miễn.
Các bước phụ thuộc nhau theo chuỗi: Điều kiện áp dụng coupon (đơn tối thiểu 500k)

phụ thuộc vào kết quả sau khi giảm giá sản phẩm - nếu đảo thứ tự hai bước này,

điều kiện kiểm tra sẽ sai đầu vào dẫn đến kết quả sai.

Loyalty Discount tính trên tổng sau coupon chứ không phải tổng gốc -

đây là quy tắc nghiệp vụ, không phải tùy chọn.
Prompt CoT là công cụ bắt buộc cho bài toán nhiều bước: Nếu dùng Zero-shot prompt

AI sẽ tự sắp xếp thứ tự theo logic mặc định của mình, không theo quy tắc nghiệp vụ cụ thể.

CoT buộc AI kiểm chứng từng bước trước khi sinh code, phát hiện sai sót ở giai đoạn phân tích

thay vì để lỗi nằm im trong code và chỉ bị phát hiện khi chạy thực tế.


Prompt CoT Tối Ưu
Hãy đóng vai một Java Senior Developer kiêm chuyên gia nghiệp vụ tài chính,
có kinh nghiệm xây dựng hệ thống tính toán đơn hàng cho thương mại điện tử.

Bối cảnh hệ thống: SpeedyCart cần tính tổng tiền đơn hàng với các quy tắc sau
(được liệt kê chưa theo thứ tự - nhiệm vụ của bạn là xác định thứ tự đúng):

Các quy tắc tính tiền:
- Giảm giá sản phẩm (Product Discount): giảm trực tiếp trên từng sản phẩm đang khuyến mãi
- Mã giảm giá (Coupon): giảm 10% tổng đơn, tối đa 100,000 VND,
  chỉ áp dụng khi tổng sau giảm giá sản phẩm >= 500,000 VND
- Chiết khấu thành viên (Loyalty Discount): Gold giảm 5%, Platinum giảm 10%,
  tính trên tổng sau khi đã trừ coupon
- Thuế VAT: 10% tính trên tổng sau tất cả chiết khấu
- Phí vận chuyển: 30,000 VND cho <= 10km, mỗi km tiếp theo cộng 5,000 VND,
  miễn phí nếu tổng sau chiết khấu và trước VAT > 1,000,000 VND

Đừng viết code ngay. Hãy thực hiện lần lượt 4 bước sau:

Bước 1 - Phân tích thứ tự đúng:
Xác định thứ tự thực hiện đúng của 5 quy tắc trên.
Giải thích tại sao mỗi bước phải đứng trước hoặc sau bước khác,
đặc biệt chỉ ra hệ quả tài chính nếu làm sai thứ tự ở bất kỳ bước nào.

Bước 2 - Liệt kê công thức từng bước:
Viết công thức tính toán chi tiết cho từng bước theo thứ tự đã xác định.
Dùng ký hiệu rõ ràng, ví dụ: subtotal_after_product_discount = ...

Bước 3 - Dry-run bằng văn bản với ca kiểm thử sau:
- Sản phẩm A: giá gốc 400,000 VND/cái, số lượng 2, đang giảm giá 10% trực tiếp
- Sản phẩm B: giá gốc 300,000 VND/cái, số lượng 1, không giảm giá
- Mã coupon: giảm 10% tổng đơn, tối đa 100,000 VND, đơn tối thiểu 500,000 VND
- Hạng thành viên: Gold (giảm 5%)
- Khoảng cách giao hàng: 12km

Tính từng bước ra số tiền cụ thể bằng văn bản, trình bày rõ từng phép tính.
Kết luận tổng tiền cuối cùng khách hàng phải trả.

Bước 4 - Sinh mã nguồn Java:
Dựa trên phân tích và dry-run đã kiểm chứng ở 3 bước trên,
sinh class OrderCalculator chứa phương thức calculateTotal(Order order).

Ràng buộc khi viết code:
- Java 17, Spring Boot 3.x
- Toàn bộ phép tính tiền dùng BigDecimal với RoundingMode.HALF_UP
- Tách mỗi bước tính toán thành một private method riêng biệt
- Không dùng magic number - tất cả hằng số phải được đặt tên rõ ràng
- Chú thích từng bước bằng tiếng Việt
- Trình bày kết quả trong một khối code markdown duy nhất

Phản Hồi Từ AI
Bước 1 - Phân tích thứ tự đúng:
THỨ TỰ ĐÚNG VÀ LÝ DO

Bước 1: Tính tổng tiền gốc → áp dụng Product Discount
Lý do: Phải có tổng tiền thực tế sau giảm giá sản phẩm trước,
vì đây là căn cứ để kiểm tra điều kiện áp dụng coupon (>= 500,000 VND).
Nếu kiểm tra coupon trên giá gốc, khách hàng có thể dùng coupon cho đơn
thực tế chỉ còn 400k sau giảm giá sản phẩm - thất thoát cho doanh nghiệp.

Bước 2: Áp dụng Coupon
Lý do: Coupon áp dụng trên tổng sau Product Discount.
Điều kiện tối thiểu 500k phải kiểm tra SAU bước 1.
Nếu tính Loyalty trước Coupon, chiết khấu thành viên sẽ bị tính trên
số tiền lớn hơn thực tế - sai nghiệp vụ.

Bước 3: Áp dụng Loyalty Discount
Lý do: Quy tắc nghiệp vụ quy định rõ Loyalty tính trên tổng SAU coupon.
Thứ tự này đảm bảo khách hàng VIP được hưởng chiết khấu trên số tiền
đã được trừ coupon, không phải trên giá gốc.

Bước 4: Kiểm tra và tính Phí vận chuyển
Lý do: Ngưỡng miễn phí ship (> 1,000,000 VND) được tính trên tổng SAU
tất cả chiết khấu và TRƯỚC VAT - đây là giá trị thực của đơn hàng.
Nếu tính VAT trước rồi mới kiểm tra ngưỡng ship, những đơn hàng gần ngưỡng
sẽ bị tính sai điều kiện miễn phí.

Bước 5: Cộng VAT (10%)
Lý do: VAT là nghĩa vụ thuế tính trên giá trị hàng hóa sau tất cả
chiết khấu thương mại - đây là quy định pháp luật thuế GTGT.
VAT không được tính trên phí vận chuyển (dịch vụ logistics tính riêng).

Bước 6: Cộng Phí vận chuyển (nếu không được miễn)
Lý do: Phí ship là khoản thu riêng, không chịu VAT hàng hóa,
phải cộng sau cùng để ra tổng tiền khách hàng thanh toán.

Bước 2 - Công thức từng bước:
CÔNG THỨC CHI TIẾT TỪNG BƯỚC

--- Bước 1: Tổng sau Product Discount ---
item_total(A) = unit_price(A) × quantity(A) × (1 - discount_rate(A))
= 400,000 × 2 × (1 - 0.10)
item_total(B) = unit_price(B) × quantity(B)
= 300,000 × 1
subtotal_after_product_discount = Σ item_total(i)

--- Bước 2: Áp dụng Coupon ---
ĐIỀU KIỆN: subtotal_after_product_discount >= 500,000
coupon_amount = MIN(subtotal_after_product_discount × coupon_rate, coupon_max_cap)
subtotal_after_coupon = subtotal_after_product_discount - coupon_amount

--- Bước 3: Áp dụng Loyalty Discount ---
loyalty_amount = subtotal_after_coupon × loyalty_rate
subtotal_after_loyalty = subtotal_after_coupon - loyalty_amount

--- Bước 4: Tính Phí vận chuyển ---
NẾU subtotal_after_loyalty > 1,000,000: shipping_fee = 0
NẾU KHÔNG:
base_distance = 10km → base_fee = 30,000
extra_distance = max(distance - 10, 0)
shipping_fee = base_fee + extra_distance × 5,000

--- Bước 5: Cộng VAT ---
vat_amount = subtotal_after_loyalty × 0.10
subtotal_after_vat = subtotal_after_loyalty + vat_amount

--- Bước 6: Tổng cuối cùng ---
order_total = subtotal_after_vat + shipping_fee

Bước 3 - Dry-run chi tiết:
DRY-RUN VỚI CA KIỂM THỬ CỤ THỂ
================================

INPUT:
- Sản phẩm A: 400,000 VND × 2 cái, giảm giá 10%
- Sản phẩm B: 300,000 VND × 1 cái, không giảm
- Coupon: 10%, tối đa 100,000 VND, đơn tối thiểu 500,000 VND
- Hạng: Gold (5%)
- Khoảng cách: 12km

BƯỚC 1 - Tính tổng sau Product Discount:
item_total(A) = 400,000 × 2 × (1 - 0.10)
= 400,000 × 2 × 0.90
= 720,000 VND
item_total(B) = 300,000 × 1 = 300,000 VND
subtotal_after_product_discount = 720,000 + 300,000 = 1,020,000 VND

BƯỚC 2 - Áp dụng Coupon:
Kiểm tra điều kiện: 1,020,000 >= 500,000 → ĐẠT
coupon_amount = MIN(1,020,000 × 10%, 100,000)
= MIN(102,000, 100,000)
= 100,000 VND  ← bị giới hạn bởi cap
subtotal_after_coupon = 1,020,000 - 100,000 = 920,000 VND

BƯỚC 3 - Áp dụng Loyalty Discount (Gold 5%):
loyalty_amount = 920,000 × 5% = 46,000 VND
subtotal_after_loyalty = 920,000 - 46,000 = 874,000 VND

BƯỚC 4 - Tính Phí vận chuyển:
Kiểm tra ngưỡng miễn phí: 874,000 <= 1,000,000 → KHÔNG miễn phí
base_fee = 30,000 VND (cho 10km đầu)
extra_distance = 12 - 10 = 2km
extra_fee = 2 × 5,000 = 10,000 VND
shipping_fee = 30,000 + 10,000 = 40,000 VND

BƯỚC 5 - Cộng VAT (10%):
vat_amount = 874,000 × 10% = 87,400 VND
subtotal_after_vat = 874,000 + 87,400 = 961,400 VND

BƯỚC 6 - Tổng cuối cùng:
order_total = 961,400 + 40,000 = 1,001,400 VND

KẾT LUẬN: Khách hàng thanh toán 1,001,400 VND

Bước 4 - Class Java OrderCalculator:
javaimport lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Service;
import java.math.BigDecimal;
import java.math.RoundingMode;

/**
* Class tính toán tổng tiền đơn hàng theo đúng thứ tự nghiệp vụ SpeedyCart.
* Thứ tự: Product Discount → Coupon → Loyalty → Shipping → VAT → Total
* Toàn bộ phép tính dùng BigDecimal để tránh sai số tiền tệ.
  */
  @Slf4j
  @Service
  public class OrderCalculator {

  // --- Hằng số nghiệp vụ - không dùng magic number trong code ---
  private static final BigDecimal COUPON_RATE          = new BigDecimal("0.10");
  private static final BigDecimal COUPON_MAX_CAP       = new BigDecimal("100000");
  private static final BigDecimal COUPON_MIN_ORDER     = new BigDecimal("500000");
  private static final BigDecimal LOYALTY_GOLD_RATE    = new BigDecimal("0.05");
  private static final BigDecimal LOYALTY_PLATINUM_RATE= new BigDecimal("0.10");
  private static final BigDecimal VAT_RATE             = new BigDecimal("0.10");
  private static final BigDecimal FREE_SHIP_THRESHOLD  = new BigDecimal("1000000");
  private static final BigDecimal BASE_SHIP_FEE        = new BigDecimal("30000");
  private static final BigDecimal EXTRA_SHIP_PER_KM    = new BigDecimal("5000");
  private static final BigDecimal BASE_SHIP_DISTANCE   = new BigDecimal("10");

  /**
    * Tính tổng tiền đơn hàng theo đúng thứ tự nghiệp vụ.
    *
    * @param order Thông tin đơn hàng đầu vào
    * @return OrderSummary chứa chi tiết từng bước và tổng cuối cùng
      */
      public OrderSummary calculateTotal(Order order) {
      log.info("Bắt đầu tính toán đơn hàng - orderId: {}", order.getOrderId());

      // Bước 1: Tính tổng tiền gốc sau khi áp dụng giảm giá từng sản phẩm
      BigDecimal subtotalAfterProductDiscount = calculateSubtotalAfterProductDiscount(order);
      log.info("Sau Product Discount: {}", subtotalAfterProductDiscount);

      // Bước 2: Áp dụng mã coupon nếu đủ điều kiện
      BigDecimal couponAmount = calculateCouponDiscount(subtotalAfterProductDiscount, order);
      BigDecimal subtotalAfterCoupon = subtotalAfterProductDiscount.subtract(couponAmount);
      log.info("Sau Coupon (trừ {}): {}", couponAmount, subtotalAfterCoupon);

      // Bước 3: Áp dụng chiết khấu thành viên tính trên tổng sau coupon
      BigDecimal loyaltyAmount = calculateLoyaltyDiscount(subtotalAfterCoupon, order);
      BigDecimal subtotalAfterLoyalty = subtotalAfterCoupon.subtract(loyaltyAmount);
      log.info("Sau Loyalty Discount (trừ {}): {}", loyaltyAmount, subtotalAfterLoyalty);

      // Bước 4: Tính phí vận chuyển dựa trên tổng sau chiết khấu và trước VAT
      BigDecimal shippingFee = calculateShippingFee(subtotalAfterLoyalty, order);
      log.info("Phí vận chuyển: {}", shippingFee);

      // Bước 5: Cộng VAT 10% tính trên tổng sau tất cả chiết khấu
      BigDecimal vatAmount = calculateVAT(subtotalAfterLoyalty);
      BigDecimal subtotalAfterVat = subtotalAfterLoyalty.add(vatAmount);
      log.info("Sau VAT (cộng {}): {}", vatAmount, subtotalAfterVat);

      // Bước 6: Cộng phí vận chuyển để ra tổng cuối cùng
      BigDecimal orderTotal = subtotalAfterVat.add(shippingFee);
      log.info("Tổng tiền cuối cùng: {}", orderTotal);

      return OrderSummary.builder()
      .subtotalAfterProductDiscount(subtotalAfterProductDiscount)
      .couponAmount(couponAmount)
      .subtotalAfterCoupon(subtotalAfterCoupon)
      .loyaltyAmount(loyaltyAmount)
      .subtotalAfterLoyalty(subtotalAfterLoyalty)
      .shippingFee(shippingFee)
      .vatAmount(vatAmount)
      .orderTotal(orderTotal)
      .build();
      }

  /**
    * Bước 1: Tính tổng tiền sau khi trừ giảm giá trực tiếp từng sản phẩm.
    * Mỗi sản phẩm có thể có discount rate riêng, hoặc 0 nếu không giảm giá.
      */
      private BigDecimal calculateSubtotalAfterProductDiscount(Order order) {
      return order.getItems().stream()
      .map(item -> {
      BigDecimal unitPrice    = item.getUnitPrice();
      BigDecimal quantity     = BigDecimal.valueOf(item.getQuantity());
      BigDecimal discountRate = item.getDiscountRate(); // 0 nếu không giảm

               // Giá sau giảm = đơn giá × (1 - tỷ lệ giảm) × số lượng
               BigDecimal discountedPrice = unitPrice
                   .multiply(BigDecimal.ONE.subtract(discountRate))
                   .setScale(0, RoundingMode.HALF_UP);

               return discountedPrice.multiply(quantity);
           })
           .reduce(BigDecimal.ZERO, BigDecimal::add);
  }

  /**
    * Bước 2: Tính số tiền giảm từ coupon.
    * Kiểm tra điều kiện tối thiểu 500,000 VND trước khi áp dụng.
    * Số tiền giảm = MIN(tổng × 10%, 100,000 VND).
      */
      private BigDecimal calculateCouponDiscount(BigDecimal subtotal, Order order) {
      if (!order.isHasCoupon()) {
      return BigDecimal.ZERO;
      }

      // Kiểm tra điều kiện giá trị đơn hàng tối thiểu
      if (subtotal.compareTo(COUPON_MIN_ORDER) < 0) {
      log.info("Đơn hàng không đủ điều kiện dùng coupon - tổng: {}, tối thiểu: {}",
      subtotal, COUPON_MIN_ORDER);
      return BigDecimal.ZERO;
      }

      // Tính số tiền giảm, không vượt quá mức tối đa 100,000 VND
      BigDecimal calculatedDiscount = subtotal
      .multiply(COUPON_RATE)
      .setScale(0, RoundingMode.HALF_UP);

      return calculatedDiscount.min(COUPON_MAX_CAP);
      }

  /**
    * Bước 3: Tính chiết khấu thành viên dựa trên hạng của khách hàng.
    * Tính trên tổng tiền SAU khi đã trừ coupon.
      */
      private BigDecimal calculateLoyaltyDiscount(BigDecimal subtotalAfterCoupon, Order order) {
      BigDecimal loyaltyRate = switch (order.getMemberTier()) {
      case GOLD     -> LOYALTY_GOLD_RATE;
      case PLATINUM -> LOYALTY_PLATINUM_RATE;
      default       -> BigDecimal.ZERO;
      };

      return subtotalAfterCoupon
      .multiply(loyaltyRate)
      .setScale(0, RoundingMode.HALF_UP);
      }

  /**
    * Bước 4: Tính phí vận chuyển dựa trên khoảng cách và tổng tiền sau chiết khấu.
    * Miễn phí nếu tổng sau chiết khấu (trước VAT) > 1,000,000 VND.
    * Phí cơ bản 30,000 VND cho 10km đầu, thêm 5,000 VND/km tiếp theo.
      */
      private BigDecimal calculateShippingFee(BigDecimal subtotalAfterLoyalty, Order order) {
      // Kiểm tra điều kiện miễn phí vận chuyển
      if (subtotalAfterLoyalty.compareTo(FREE_SHIP_THRESHOLD) > 0) {
      log.info("Đơn hàng được miễn phí vận chuyển - tổng sau chiết khấu: {}",
      subtotalAfterLoyalty);
      return BigDecimal.ZERO;
      }

      BigDecimal distance = BigDecimal.valueOf(order.getDistanceKm());

      // Tính số km vượt quá ngưỡng cơ bản 10km
      BigDecimal extraDistance = distance.subtract(BASE_SHIP_DISTANCE)
      .max(BigDecimal.ZERO);

      BigDecimal extraFee = extraDistance.multiply(EXTRA_SHIP_PER_KM);

      return BASE_SHIP_FEE.add(extraFee);
      }

  /**
    * Bước 5: Tính VAT 10% trên tổng tiền sau tất cả chiết khấu thương mại.
    * VAT không tính trên phí vận chuyển theo quy định thuế GTGT.
      */
      private BigDecimal calculateVAT(BigDecimal subtotalAfterAllDiscounts) {
      return subtotalAfterAllDiscounts
      .multiply(VAT_RATE)
      .setScale(0, RoundingMode.HALF_UP);
      }
      }