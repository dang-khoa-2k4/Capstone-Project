# Script thuyet trinh - Motion Planning su dung toi uu loi

Muc tieu thoi luong: 20-25 phut.  
Nhip de xuat: noi ro cac y chinh, khong doc het cong thuc; dung hinh va bang de dan cau chuyen.

## 1. Trang bia

**Thoi luong:** 20-30 giay

Kinh chao quy thay co va cac ban. Nhom em xin trinh bay de tai "Hoach dinh chuyen dong cua robot su dung toi uu loi". De tai tap trung vao bai toan tim quy dao cho robot trong moi truong co vat can, trong do chung em khai thac framework Graph of Convex Sets, hay GCS, va mo rong theo huong ket hop voi Direct Multiple Shooting cho dong luc hoc phi tuyen.

Noi dung trinh bay gom phan dong luc va mo hinh bai toan, co so ly thuyet GCS, cong thuc toi uu hoa, cac thuc nghiem, va cuoi cung la tong ket cung huong phat trien.

## 2. Tong quan

**Thoi luong:** 20-30 giay

Day la cau truc bai trinh bay. Dau tien em se gioi thieu vi sao bai toan hoach dinh chuyen dong can ket hop quyet dinh roi rac va lien tuc. Sau do em trinh bay y tuong Graph of Convex Sets: bien khong gian an toan thanh cac vung loi va giai bai toan duong di ngan nhat tren do thi. Tiep theo la phan toi uu hoa, gom formulation GCS-Bezier va GCS-DMS. Cuoi cung, em di vao thuc nghiem de chi ra diem manh, han che va vai tro bo tro cua hai formulation nay.

## 3. Dong luc

**Thoi luong:** 50-60 giay

Trong robot, nhieu bai toan khong chi la "di tu diem A den diem B". Robot phai chon di qua vung nao, tranh vat can nao, dong thoi phai tao duoc quy dao lien tuc, tron, va thoa cac rang buoc vat ly.

Noi cach khac, bai toan co hai tang quyet dinh. Tang tren la roi rac: chon hanh lang, chon thu tu cac vung an toan, chon duong di trong moi truong. Tang duoi la lien tuc: toa do, van toc, dieu khien, thoi gian va do tron cua quy dao.

Hai hinh tren slide minh hoa hai boi canh: robot bon chan di tren dia hinh phuc tap va quy dao toi uu trong moi truong dang me cung. Diem chung la neu chi dung duong di hinh hoc thi chua du; ta can mot cong cu co the ket hop lua chon roi rac voi toi uu lien tuc.

## 4. Bai toan hoach dinh chuyen dong

**Thoi luong:** 50-60 giay

Bai toan motion planning co dau vao la trang thai ban dau, trang thai dich va tap vat can. Trang thai co the chi la vi tri, nhung voi robot that thi thuong gom ca huong, van toc, va cac bien dong luc hoc khac.

Dau ra mong muon la mot quy dao theo thoi gian, hoac mot chuoi dieu khien de robot bam theo quy dao do. Quy dao nay phai tranh va cham, du tron, va toi uu theo mot chi phi nao do, vi du do dai, thoi gian, nang luong hoac do cong.

Hinh ben phai cho thay su khac nhau giua path planning va motion planning. Path planning chu yeu tim duong hinh hoc trong khong gian. Motion planning yeu cau them yeu to thoi gian va rang buoc vat ly, nen kho hon va gan voi robot thuc te hon.

## 5. GCS vuot troi ve toi uu toan cuc va tich hop rang buoc dong hoc

**Thoi luong:** 60 giay

Cac cach tiep can truyen thong thuong tach bai toan thanh hai buoc: tim duong di roi moi toi uu hoa quy dao. Cach nay don gian va de cai dat, nhung co the mat toi uu toan cuc vi quyet dinh duong di da bi co dinh qua som.

Nhom sampling-based nhu RRT va PRM co uu diem la tim duong nhanh trong khong gian lon, nhung quy dao thu duoc thuong khong tron, khong toi uu, va can hau xu ly neu muon thoa rang buoc dong hoc.

GCS tiep can khac hon: ta bieu dien moi truong bang do thi cua cac tap loi an toan, sau do giai bai toan duong di ngan nhat co kem bien lien tuc. Nhu vay, viec chon duong di va viec toi uu quy dao duoc dat trong cung mot framework. Day la ly do GCS phu hop voi muc tieu cua de tai.

## 6. Y tuong chinh cua GCS

**Thoi luong:** 60 giay

Y tuong cot loi cua GCS la phan hoach khong gian tu do thanh cac vung an toan loi, ky hieu la Q_v. Moi vung loi nay tro thanh mot dinh trong do thi. Neu hai vung co the chuyen tiep voi nhau, ta them mot canh giua hai dinh.

Khi do, bai toan motion planning duoc dua ve bai toan shortest path problem tren Graph of Convex Sets. Phan roi rac la chon canh nao, tuc la chon chuoi vung nao de di qua. Phan lien tuc la chon trang thai hoac quy dao ben trong tung vung.

Diem quan trong la nhung rang buoc ben trong tung vung la rang buoc loi. Nho do, sau khi relax bien roi rac, bai toan co the dua ve convex optimization va trong nhieu truong hop cho nghiem rat chat. Pipeline ben duoi tom tat qua trinh: moi truong, phan hoach loi, lap do thi, roi toi uu tren GCS.

## 7. Phan hoach loi chia C_free thanh cac vung khong va cham

**Thoi luong:** 50 giay

Buoc dau tien de dung GCS la phan hoach khong gian tu do C_free thanh mot tap huu han cac vung loi Q_v. Moi vung phai nam trong phan khong va cham, va ly tuong la cac vung nay phu duoc toan bo khong gian tu do.

Ve mat thiet ke, ta muon so vung cang it cang tot, nhung moi vung lai cang lon cang tot. Neu qua nhieu vung, do thi GCS lon va bai toan toi uu hoa cham. Neu qua it hoac phan hoach qua tho, ta co the bo sot cac hanh lang hep va lam giam chat luong quy dao.

Trong GCS, chat luong phan hoach anh huong truc tiep den chat luong nghiem va thoi gian giai. Vi vay day khong phai buoc tien xu ly phu, ma la mot thanh phan quan trong cua toan bo framework.

## 8. Mot so giai thuat phan hoach loi

**Thoi luong:** 45-55 giay

Co nhieu cach tao cac vung loi. Nhom phan hoach chinh xac nhu trapezoidation hay giai thuat Seidel co the cho phan hoach hinh hoc ro rang trong 2D, nhung co the tao nhieu vung nho.

Nhom phan hoach xap xi nhu ACD va VCC huong den viec tao it vung hon, lon hon, va phu hop hon cho bai toan toi uu. ACD chia vat the hoac khong gian thanh cac thanh phan gan loi. VCC thi dua tren y tuong visibility: neu cac diem trong khong gian tu do nhin thay nhau, chung co kha nang thuoc cung mot vung loi.

Trong thuc nghiem, chung em so sanh manual, ACD va VCC de xem phan hoach tac dong nhu the nao den GCS-Bezier va GCS-DMS.

## 9. Cau truc GCS

**Thoi luong:** 50-60 giay

Ve mat toan hoc, GCS la mot do thi co huong G bang V va E. Moi dinh v gan voi mot bien lien tuc x_v, va bien nay nam trong mot tap loi X_v. Trong bai toan motion planning, X_v dai dien cho cac diem dieu khien hoac trang thai nam trong vung an toan.

Moi canh e tu u sang v co hai thanh phan. Thu nhat la ham chi phi l_e, phu thuoc vao bien lien tuc o hai dau canh. Thu hai la rang buoc ket noi X_e, dam bao quy dao noi tu vung nay sang vung kia mot cach hop le.

Noi ngan gon, GCS khong chi la do thi roi rac. Moi dinh va moi canh deu mang theo mot bai toan lien tuc loi, nen no ket hop duoc ca cau truc do thi va toi uu hoa hinh hoc.

## 10. Tiep can 1: Bai toan toi uu hoa trong GCS

**Thoi luong:** 60 giay

Slide nay ghep hai nhin nhan cua bai toan. Ben trai la phan roi rac. Bien y_e bang 1 neu canh e duoc chon, va bang 0 neu khong. Rang buoc bao toan luong dam bao ta co mot dong chay tu dinh nguon sigma den dinh dich tau.

Ben phai la phan lien tuc. Ta muon toi uu quy dao q(t), trong do chi phi co the gom thoi gian, do dai duong di, va nang luong hay do min cua van toc. Dong thoi quy dao phai nam trong hop cac vung an toan, thoa rang buoc van toc va dieu kien bien.

GCS gom hai phan nay thanh mot muc tieu tong: toi thieu tong chi phi tren cac canh duoc chon. Diem hay la chi phi moi canh van phu thuoc vao bien lien tuc, nen duong di tot khong chi la it canh hay ngan ve hinh hoc, ma con tot theo chi phi quy dao.

## 11. Hoach dinh chuyen dong su dung mo hinh SPP trong GCS - bien trang thai

**Thoi luong:** 55-65 giay

Trong formulation GCS-Bezier, muc tieu la tim quy dao toi uu tu sigma den tau tren do thi GCS. Dau tien, moi truong 2D duoc phan thanh cac vung loi an toan Q_v. Sau do ta tao do thi lien ke dua tren cac vung co giao nhau hoac co kha nang chuyen tiep.

Voi moi dinh v, chung em gan bien x_v gom ba diem: diem vao, diem giua va diem ra cua doan quy dao trong vung do. Vi moi diem co toa do x, y nen x_v nam trong R^6. Rang buoc X_v yeu cau ca ba diem nay deu nam trong vung Q_v.

Cach bieu dien nay giup moi vung mang mot doan quy dao cuc bo. Khi cac vung duoc noi voi nhau, cac diem vao-ra se tao thanh mot quy dao lien tuc tren toan bo moi truong.

## 12. Hoach dinh chuyen dong su dung mo hinh SPP trong GCS - rang buoc canh

**Thoi luong:** 55-65 giay

Sau khi co bien tren dinh, ta can rang buoc tren canh de cac doan quy dao ghep lai duoc. Rang buoc C0 yeu cau diem ra cua vung truoc bang diem vao cua vung sau. Dieu nay dam bao quy dao khong bi dut doan ve vi tri.

Rang buoc C1 yeu cau huong dao ham o diem noi khop nhau, tuc la robot khong doi huong dot ngot khi di qua bien giua hai vung. Khi dung Bezier, cac rang buoc lien tuc nay co the viet thanh rang buoc tuyen tinh tren diem dieu khien.

Ham chi phi canh la tich phan binh phuong van toc, nen uu tien quy dao min va tiet kiem nang luong hinh hoc. Nhu vay formulation nay rat manh cho bai toan toi uu toan cuc hinh hoc, dac biet khi cac rang buoc deu giu duoc tinh loi.

## 13. GCS-DMS gop graph problem va OCP cuc bo thanh MINLP

**Thoi luong:** 65-75 giay

Phan tiep theo la huong mo rong cua chung em: GCS-DMS. Ly do can DMS la GCS-Bezier rat tot cho quy dao hinh hoc, nhung voi robot co dong luc hoc phi tuyen, vi du unicycle, ta can dam bao quy dao thoa phuong trinh dong luc hoc.

Ben trai la graph problem. Bien y_uv cho biet canh nao duoc chon, bien p_v cho biet vung nao duoc kich hoat. Bao toan luong dam bao chuoi vung tao thanh duong tu dau den dich. Tai giao dien giua hai vung, bien z_uv dam bao trang thai ra cua vung truoc va trang thai vao cua vung sau khop nhau.

Ben phai la OCP tren tung vung active. Moi vung co thoi luong Delta_v, dieu khien u_v va chi phi cuc bo gom thoi gian, van toc hinh hoc va nang luong dieu khien. Vi bai toan vua co bien nhi phan, vua co OCP phi tuyen, nen no la MIOCP; sau khi roi rac hoa dieu khien bang DMS thi thanh MINLP.

## 14. DMS tham so hoa doan quy dao va defect constraint

**Thoi luong:** 60 giay

Trong Direct Multiple Shooting, moi vung active duoc xem nhu mot shooting block. Cac bien quyet dinh gom trang thai vao s_v^-, trang thai ra s_v^+, thoi luong Delta_v, va tham so dieu khien w_v.

Tu trang thai vao va dieu khien, ta tich phan phuong trinh dong luc hoc tren thoi gian chuan hoa tau tu 0 den 1. Ket qua tich phan tai cuoi doan duoc goi la endpoint map F_v.

Rang buoc quan trong nhat la defect constraint: s_v^+ tru F_v bang 0. Nghia la trang thai ra doc lap ma optimizer chon phai trung voi trang thai sinh ra boi dong luc hoc. Uu diem cua multiple shooting la cac doan co the toi uu dong thoi, thay vi phai tich phan tuan tu mot chuoi dai.

## 15. Giai MINLP va rang buoc an toan

**Thoi luong:** 65-75 giay

Quy trinh giai duoc chia thanh hai pha. Pha mot chon duong di: relax bien y_uv tu nhi phan sang khoang 0-1, giai convex problem tren toan GCS, sau do lam tron de co chuoi vung active.

Pha hai toi uu quy dao dong luc hoc. Ta dung ket qua pha mot lam warm-start, roi toi uu dong thoi cac bien s_v^-, s_v^+, w_v va Delta_v tren cac vung active. Luc nay bai toan la NLP phi loi nen khong con bao dam toi uu toan cuc, nhung co the cho nghiem kha thi dong luc hoc.

Ve an toan continuous-time, chung em thu hai co che. Lay mau tai cac node don gian va nhanh, nhung co the co blind spot giua hai node. Log-barrier mo hinh hoa lien tuc hon, day quy dao nam sau trong vung an toan, nhung cham va nhay voi tham so mu.

## 16. Thiet lap thuc nghiem

**Thoi luong:** 50-60 giay

Trong phan thuc nghiem, chung em chay hai formulation trong cung cau hinh may. GCS-Bezier duoc giai bang Drake va Mosek theo dang SOCP. GCS-DMS duoc cai dat bang Python, CasADi va IPOPT cho bai toan NLP.

Co ba nhom thuc nghiem. TN1 kiem tra GCS-DMS voi robot unicycle trong moi truong 2D va me cung 15 nhan 15. TN2 danh gia kha nang mo rong to hop cua GCS-Bezier tren me cung 25 nhan 25. TN3 so sanh tac dong cua cac thuat toan phan hoach loi: manual, ACD va VCC.

Muc tieu khong phai xep hang truc tiep hai formulation, vi chung giai hai muc tieu khac nhau. GCS-Bezier nhan manh toi uu toan cuc hinh hoc, con GCS-DMS nhan manh tinh kha thi dong luc hoc.

## 17. TN3 - Ba phuong phap phan hoach loi

**Thoi luong:** 50-60 giay

Truoc het la TN3, voi cung mot moi truong 2D va cung diem dau-diem dich. Chung em so sanh ba cach phan hoach.

Manual co 12 vung, day la phan hoach do con nguoi tao nen khong tinh thoi gian decompose. ACD tao 15 vung chi trong 0.01 giay, kha gan voi manual. VCC tao khoang 33 vung, phu 96 phan tram khong gian, nhung mat 5.78 giay cho phan hoach.

Nhan xet ban dau la ACD cho can bang tot giua tu dong hoa va do gon cua do thi. VCC tao cau truc day hon, co the huu ich trong khong gian phuc tap, nhung voi bai 2D nay no lam so canh GCS tang va co nguy co lam bai toan giai cham hon.

## 18. TN3 - Tac dong len GCS-Bezier

**Thoi luong:** 60-70 giay

Khi dua ba phan hoach vao GCS-Bezier, ta thay trade-off kha ro. Manual giai nhanh nhat, khoang 3.9 giay, nhung path cost la 27.88 va khong tu dong nen kho ap dung cho robot tu hanh.

ACD giai trong 4.31 giay, gan bang manual, nhung path cost giam xuong 24.63. VCC cho path cost tot nhat, 24.44, nhung thoi gian solve tang len gan 270 giay, tong cong hon 275 giay neu tinh ca phan hoach.

Ket luan o day la ACD can bang nhat cho moi truong 2D nay: tu dong, chi phi duong di gan VCC, nhung nhanh hon VCC khoang 64 lan. VCC cho thay neu phan hoach qua day, so bien nhi phan va so canh tang co the lam chi phi toi uu hoa tang rat manh.

## 19. TN3 - Tac dong len GCS-DMS Unicycle

**Thoi luong:** 60-70 giay

Voi GCS-DMS, ket qua co khac mot chut. Manual van co cost thap nhat, 47.35, va solve nhanh nhat 17.38 giay. ACD co 9 vung active, cost 51.55 va solve 34.23 giay. VCC co 33 vung toan cuc nhung chi 7 vung active, solve 26.36 giay, tuy cost cao hon.

Diem dang chu y la voi DMS, so vung toan cuc khong phai luc nao cung quyet dinh tat ca. So vung active va chat luong chuoi vung moi anh huong truc tiep hon den NLP sau cung.

Ve tinh kha thi, cac chi so defect, gap va vi pham an toan deu rat nho, o muc 10^-8 den 10^-14. Dieu nay cho thay formulation DMS co the tao quy dao thoa dong luc hoc va lien tuc voi do chinh xac so hoc tot.

## 20. TN2 - Me cung 25x25: Linear vs Bezier GCS

**Thoi luong:** 55-65 giay

TN2 kiem tra kha nang mo rong to hop cua GCS-Bezier tren me cung 25 nhan 25, tuc 625 o. Chung em chay 10 me cung DFS ngau nhien, moi me cung loai bo 100 tuong.

Bang ben phai so sanh hai cach tham so hoa: Linear va Bezier. Linear giai rat nhanh, trung binh 1.28 giay, nhung chi cho duong gap khuc. Bezier bac 6 voi C2 cham hon nhieu, trung binh 85.22 giay, va cost cao hon do no phai om cua rong de dam bao do tron.

Tuy nhien diem quan trong la tren ca 10 me cung, relaxation deu chat: y_e tu dong ve 0-1 va optimality gap bang 0 phan tram. Dieu nay cho thay GCS co kha nang xu ly bai toan to hop lon mot cach on dinh trong cau truc me cung 2D.

## 21. TN2 - Vi sao GCS xu ly duoc 625 o?

**Thoi luong:** 55-65 giay

Ly do GCS xu ly duoc 625 o nam o cach no ma hoa bien nhi phan. Voi MICP truyen thong, neu moi doan quy dao phai gan vao mot trong |I| vung, so bien nhi phan co the tang theo |I| binh phuong. Voi 625 o, con so nay la 390625 bien, gan nhu khong kha thi.

GCS dung network flow. No chi tao bien cho cac canh giua nhung vung ke nhau. Trong me cung 2D, bac do thi thap, nen so canh xap xi 2 lan so vung. Voi 625 o, ta chi can khoang 1250 bien.

Day la khac biet cot loi: GCS khai thac cau truc hinh hoc cua khong gian, nen bien nhi phan tang theo so lien ket cuc bo thay vi tang theo moi cap vung co the co.

## 22. TN1 - GCS-DMS hoi tu tren moi truong 2D

**Thoi luong:** 55-65 giay

TN1 danh gia GCS-DMS voi robot unicycle. Trong moi truong 2D dau tien, bai toan kich hoat 9 vung, thoi gian thuc thi quy dao la 25.47 giay, thoi gian solve la 17.94 giay, va cost la 51.55.

Dieu quan trong hon la cac chi so kha thi. Defect dynamics o muc 2.47 nhan 10^-11, connection gap o muc 10^-10, va vi pham an toan o muc 10^-8. Cac gia tri nay cho thay nghiem thoa dong luc hoc, tinh lien tuc giua cac vung, va rang buoc an toan voi sai so rat nho.

Hinh ben trai minh hoa qua trinh hoi tu: qua cac vong lap IPOPT, defect va connection gap giam dan cho den khi quy dao tro thanh lien tuc va kha thi.

## 23. TN1 - GCS-DMS tren me cung 15x15

**Thoi luong:** 55-65 giay

Tiep theo chung em chay GCS-DMS tren me cung 15 nhan 15 voi nhieu seed. Robot la unicycle, co dong luc hoc p_x cham bang v cos theta, p_y cham bang v sin theta, va theta cham bang omega. Van toc v bi gioi han trong [-2, 2], va omega bi gioi han boi pi.

Ket qua la 10 tren 10 seed thanh cong voi ca hai co che an toan: log-barrier va luoi diem huu han. Defect dat khoang 10^-10, gap khoang 10^-9 va vi pham an toan khoang 10^-8.

Dieu nay cho thay GCS-DMS khong chi dung duoc trong moi truong don gian, ma con tao duoc quy dao kha thi dong luc hoc tren cac cau truc me cung khac nhau.

## 24. TN1 - Ho so van toc va dau vet dong luc phi tuyen

**Thoi luong:** 55-65 giay

Slide nay cho thay ly do DMS can thiet khi xet robot that. Ho so van toc cua unicycle dao dong khoang 0.6 den 0.75 met tren giay, va giam sau tai cac nut giao, hanh lang hep, hoac doan phai chuyen qua nhieu vung.

Hanh vi nay khong phai do ta lap trinh truc tiep, ma xuat hien tu rang buoc dong luc hoc. Voi unicycle, khi toc do quay omega bi gioi han, robot muon cua gap thi phai giam van toc tien v, vi ban kinh cua lien quan den v chia cho omega max.

Day la diem GCS-Bezier hinh hoc thuan khong phan anh duoc. Bezier co the tao duong tron, nhung khong tu dong sinh ra ho so van toc phu hop voi dong luc phi tuyen neu khong mo hinh hoa dong luc hoc.

## 25. TN1 - Log-barrier vs luoi diem

**Thoi luong:** 60-70 giay

Cuoi cung trong TN1, chung em so sanh hai co che an toan tren cung 10 seed me cung 15 nhan 15.

Log-barrier thanh cong 10 tren 10, cost trung binh 118.63, nhung solve trung binh 245.51 giay. Uu diem cua no la day quy dao nam sau trong vung an toan va co dien giai ly thuyet tot. Nhuoc diem la ham log phi tuyen lam bai toan cham hon.

Luoi huu han diem cung thanh cong 10 tren 10, solve nhanh hon nhieu, trung binh 81.60 giay, cost 115.55 va integrality gap nho hon. Doi lai, dam bao an toan phu thuoc vao so luong va vi tri diem lay mau.

Vi vay hai co che phuc vu hai uu tien khac nhau: log-barrier nghieng ve mo hinh hoa lien tuc va ly thuyet; luoi diem nghieng ve toc do va tinh thuc dung.

## 26. GCS-Bezier va GCS-DMS bo tro nhau

**Thoi luong:** 60-70 giay

Tu ba nhom thuc nghiem, chung em rut ra ba ket luan chinh. Thu nhat, voi phan hoach, ACD can bang nhat trong moi truong 2D; VCC co the cho vung day hon nhung lam bai toan giai cham; va chat luong phan hoach anh huong truc tiep den chi phi giai.

Thu hai, voi kha nang mo rong to hop, GCS-Bezier xu ly me cung 625 o on dinh, relaxation chat va gap bang 0 phan tram. Doi lai, neu yeu cau Bezier C2 thi chi phi tang do quy dao phai tron hon.

Thu ba, voi dong luc hoc phi tuyen, GCS-DMS tao duoc quy dao kha thi cho unicycle voi defect rat nho. Tuy nhien vi pha DMS la NLP phi loi, no khong bao dam toi uu toan cuc nhu GCS-Bezier.

Ket luan chung la hai huong nay bo tro nhau: Bezier manh ve toi uu toan cuc hinh hoc, DMS manh ve kha thi dong luc hoc tren robot thuc.

## 27. Han che: GCS phu thuoc phan hoach va moi truong dong

**Thoi luong:** 50-60 giay

GCS van co mot so han che quan trong. Dau tien la phu thuoc vao phan hoach. Neu so vung tang, kich thuoc bai toan va chi phi tinh toan tang. Neu phan hoach qua tho, co the bo sot vung tu do nho hoac hanh lang hep.

Thu hai la ban chat to hop cua bai toan. Ve ly thuyet, so bien nhi phan ti le voi so canh va bai toan van NP-hard. Convex relaxation giup rat nhieu trong thuc te, nhung khong bao dam luon chat trong moi truong hop.

Thu ba la moi truong dong. Neu vat can di chuyen, ta co the phai tai phan hoach va giai lai bai toan. Voi thoi gian giai hien tai, dac biet tren Bezier lon hoac DMS, framework nay chua phu hop cho real-time neu khong co them chien luoc tang toc.

## 28. Tong ket va huong nghien cuu tuong lai

**Thoi luong:** 60-75 giay

De tong ket, trong de tai nay chung em da mo hinh hoa bai toan hoach dinh chuyen dong quanh vat can bang toi uu loi, cu the la Graph of Convex Sets. Chung em da tai hien cau truc GCS cho khong gian 2D, so sanh cac phuong phap phan hoach, va kiem chung kha nang toi uu cua GCS-Bezier.

Ben canh do, chung em mo rong theo huong GCS-DMS de dua dong luc hoc phi tuyen vao bai toan. Ket qua thuc nghiem cho thay formulation nay co the tao quy dao kha thi cho unicycle voi sai so defect, gap va vi pham an toan rat nho.

Huong phat trien tiep theo gom ba diem. Mot la hoan thien mo hinh cho khong gian cao chieu. Hai la tich hop va kiem thu tren robot thuc. Ba la benchmark quy mo lon voi RRT*, PRM va cac phuong phap toi uu cuc bo de danh gia ro hon ve toc do, chat luong quy dao va do on dinh.

## 29. Tai lieu tham khao

**Thoi luong:** 10-15 giay

Day la cac tai lieu chinh ma chung em dua vao, dac biet la cong trinh cua Marcucci va cong su ve motion planning around obstacles with convex optimization, bai shortest paths in Graphs of Convex Sets, cung cac tai lieu ve IRIS, VCC, PRM, RRT va Bezier.

Neu quy thay co can doi chieu them, cac cong thuc va ket qua trong slide deu duoc gan citation tu nhung nguon nay.

## 30. Cam on

**Thoi luong:** 10-20 giay

Phan trinh bay cua nhom em den day la ket thuc. Chung em xin cam on quy thay co va cac ban da lang nghe. Nhom em rat mong nhan duoc cau hoi va gop y.

# Nhac nho khi tap noi

- Neu bi thieu thoi gian, rut ngan slide 7, 8, 17 va 29.
- Neu con nhieu thoi gian, mo rong slide 13-15 vi day la phan dong gop GCS-DMS.
- Khi gap slide co cong thuc, chi noi y nghia cua bien va rang buoc; khong doc tung dong.
- Appendix nen de tra loi cau hoi, khong dua vao 20-25 phut chinh.

# Script ngan cho appendix neu duoc hoi

## Do tron cua quy dao

Do tron C0 nghia la lien tuc ve vi tri, C1 la lien tuc ve van toc, C2 la lien tuc ve gia toc. Trong robot, do tron quan trong vi no quyet dinh viec robot co doi huong hoac doi luc dot ngot hay khong. Bezier thuan tien vi rang buoc C1, C2 co the viet qua sai phan cac diem dieu khien.

## GCS vs MICP truyen thong

Khac biet chinh nam o so bien nhi phan. MICP truyen thong gan tung doan quy dao vao mot vung, nen so bien co the tang theo binh phuong so vung. GCS chi tao bien tren cac canh lien ke cua do thi, nen trong 2D so bien thuong tang gan tuyen tinh theo so vung.

## Bezier

Bezier co tinh chat bao loi: neu tat ca diem dieu khien nam trong mot vung loi an toan, toan bo duong cong nam trong vung do. Day la ly do Bezier rat hop voi GCS, vi rang buoc an toan lien tuc duoc thay bang rang buoc huu han tren diem dieu khien.

## VCC

VCC dua tren visibility graph. Ta lay mau cac diem trong khong gian tu do, noi hai diem neu doan thang giua chung khong va cham, roi tim cac clique lon. Moi clique dai dien cho mot tap diem co kha nang thuoc cung mot vung loi, sau do duoc inflate thanh da dien an toan.

## Convex relaxation va rounding

Ban dau GCS co bien nhi phan y_e nen la bai toan hon hop nguyen. Relaxation cho phep y_e nam trong khoang 0-1 va dung ky thuat perspective de giu tinh loi. Neu nghiem relax khong nguyen, ta rounding bang cach chon canh theo xac suat ti le voi y_e sao cho tao duoc duong tu nguon den dich.

## DMS-GCS

Trong DMS-GCS, GCS chon skeleton roi rac, con multiple shooting bien moi vung active thanh mot doan dong luc hoc. Rang buoc defect dam bao trang thai ra cua moi doan khop voi ket qua tich phan phuong trinh dong luc hoc, va rang buoc interface dam bao cac doan noi lien tuc voi nhau.
