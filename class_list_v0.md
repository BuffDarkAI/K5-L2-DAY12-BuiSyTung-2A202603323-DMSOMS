# Danh sách lớp v0 — Bùi Sỹ Tùng · G07

Mỗi lớp trả lời: model cần nhận ra cái này để làm gì? Cột cuối ghi theo ảnh hồng ngoại (IR), vì camera cabin ban đêm chỉ cho ảnh đen trắng.

## DMS (7 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | eye_left, eye_right (gán riêng từng mắt) | open / closed / unknown | Phát hiện buồn ngủ qua mắt nhắm kéo dài. Gán riêng từng mắt để khi một mắt bị che thì mắt còn lại vẫn dùng được | Có khi thấy rõ khe mắt. Mất khi mặt nghiêng, vùng tối hoặc kính phản xạ (D04, D06 ở `ir_log`) |
| 2 | gaze_zone | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát hiện tài xế nhìn đi chỗ khác (mất tập trung), và nhắc nhở khi xe cần người lái tiếp quản | Có, suy từ hướng đầu và mắt. Khó hơn khi mắt không thấy |
| 3 | glasses | none / clear / sun / unknown | Chọn cách đọc mắt cho phù hợp, vì kính râm hoặc kính phản xạ làm model đọc mắt kém. Cũng để không báo nhầm "mắt nhắm" khi chỉ là không thấy | Một phần. Trên NIR kính râm có thể trông trong suốt nên dễ ghi nhầm |
| 4 | face13 (13 điểm mặt, cờ `occluded` / `outside`) | tọa độ 13 điểm + cờ từng điểm | Tính độ mở mắt, tư thế đầu, ngáp. Cờ cho model biết điểm nào không đáng tin | Có, nhưng điểm mắt và miệng hay bị che (D03 chai nước che miệng) |
| 5 | hand (`whose`, `side`, `on_wheel`) | driver/passenger · left/right · yes/no/unknown | Kiểm tra tay có trên vô lăng không, và phát hiện tay đang cầm vật khác khi lái | Có, nhận bằng hình dạng. Tay ngoài vùng sáng đèn IR thì tối |
| 6 | phone (`state`) | held / mounted / lap | Phát hiện dùng điện thoại khi lái | Có khi thấy cạnh hoặc màn hình máy. Tay cầm vật tối mà không thấy hình dạng thì không kết luận là điện thoại |
| 7 | person (`seatbelt`) của tài xế | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây an toàn | Khó trên IR: dây tối trên áo tối thì không phân biệt được, nên cần `unknown` |

## OMS (6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | person + seat | 5 vị trí ghế theo xe | Đếm người và biết ghế nào có người để nhắc dây an toàn và điều khiển túi khí | Có, nhận bằng hình dạng. Camera nhìn về sau nên trái ảnh là phải xe |
| 2 | seatbelt (người ngồi sau) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây cho hành khách hàng sau | Khó: dây tối chìm trong bóng tối, không đoán từ màu |
| 3 | child_seat (`occupied`) | no / unknown | Nhận biết có lắp ghế trẻ em để bật chế độ phù hợp (ví dụ khóa cửa trẻ em, tắt túi khí) | Có, nhận bằng hình dạng ghế. Ghế bị phủ vải thì phải ghi `unknown` |
| 4 | infant_seat (`facing`) | rear / front | Ghế em bé quay về sau thì phải tắt túi khí ghế trước. Nhắc quên trẻ trong xe | Có, nhận bằng hình dạng. Hướng quay khó xác định khi nhìn từ xa |
| 5 | object (`type`) | bag / box / cup / other | Nhắc quên đồ trong xe, phát hiện vật nặng chưa cố định có thể bay khi phanh gấp | Có, nhận bằng hình dạng và vị trí. Không nhận bằng màu |
| 6 | face (người ngồi sau, chỉ box) | có / không | Biết người ngồi sau có quay mặt về camera không, hỗ trợ đếm người và phát hiện người nằm hoặc cúi | Có khi thấy mặt. Tóc che mặt thì không có box |

## 2 câu hỏi leo thang

1. Câu hỏi: Khi chỉ nhìn thấy ghế từ xa hoặc ở góc nghiêng, tiêu chí nào phân biệt `child_seat` với `infant_seat`? Guideline chỉ định nghĩa "ghế trẻ em" và "ghế em bé (nôi xách)" bằng tên gọi, không nêu dấu hiệu hình dạng (có quai xách, ngả về sau).
   Khung ví dụ: S02 (ghế bên trái ảnh, tức `rear_right`). Hai người gán cho cùng vật này hai lớp khác nhau.
   Bạn tạm gán thế nào và vì sao: Tạm gán `child_seat`, `occupied = no`, vì thấy ghế có dạng ghế ngồi có lưng chứ không phải nôi xách. Nếu hướng dẫn ghi rõ dấu hiệu nhận dạng thì sẽ gán đồng nhất hơn.
2. Câu hỏi: Khi mắt bị che bởi bóng tối hoặc do mặt nghiêng, điểm mắt trong `face13` nên đánh `occluded` hay `outside`? Guideline ghi `occluded` nếu đoán chắc vị trí và `outside` nếu không đoán được, nhưng "đoán chắc" chưa có ngưỡng cụ thể nên mỗi người hiểu một kiểu.
   Khung ví dụ: D06 (mắt trái trong vùng tối). Hai người đánh cờ ngược nhau: một bên `outside`, một bên `occluded`.
   Bạn tạm gán thế nào và vì sao: Tạm gán `outside` cho 4 điểm mắt trái và `eye_left = unknown`, vì không đoán được vị trí mí và khóe mắt trong vùng tối. Đánh `occluded` mà đặt điểm sai chỗ thì dạy model sai.
