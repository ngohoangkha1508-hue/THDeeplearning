#import pandas as pd
import numpy as np
import os

# Tạo dữ liệu giả lập cho tệp dulieuxettuyendaihoc.csv
data = {
    'STT': range(1, 11),
    'DT': [1, np.nan, 0, 1, np.nan, 0, 1, 0, np.nan, 1],
    'KT': ['A', 'A1', 'B', 'C', 'D', 'A', 'B', 'A1', 'C', 'D'],
    'T1': np.random.uniform(5, 10, 10), 'L1': np.random.uniform(5, 10, 10), 'H1': np.random.uniform(5, 10, 10), 
    'S1': np.random.uniform(5, 10, 10), 'V1': np.random.uniform(5, 10, 10), 'X1': np.random.uniform(5, 10, 10), 
    'D1': np.random.uniform(5, 10, 10), 'N1': np.random.uniform(5, 10, 10),
    'T2': np.random.uniform(5, 10, 10), 'L2': np.random.uniform(5, 10, 10), 'H2': np.random.uniform(5, 10, 10), 
    'S2': np.random.uniform(5, 10, 10), 'V2': np.random.uniform(5, 10, 10), 'X2': np.random.uniform(5, 10, 10), 
    'D2': np.random.uniform(5, 10, 10), 'N2': np.random.uniform(5, 10, 10),
    'T6': np.random.uniform(5, 10, 10), 'L6': np.random.uniform(5, 10, 10), 'H6': np.random.uniform(5, 10, 10), 
    'S6': np.random.uniform(5, 10, 10), 'V6': np.random.uniform(5, 10, 10), 'X6': np.random.uniform(5, 10, 10), 
    'D6': np.random.uniform(5, 10, 10), 'N6': np.random.uniform(5, 10, 10),
    'DH1': np.random.uniform(2, 9, 10), 'DH2': np.random.uniform(2, 9, 10), 'DH3': np.random.uniform(2, 9, 10)
}

df_sample = pd.DataFrame(data)
df_sample.to_csv('dulieuxettuyendaihoc.csv', index=False)
print("Đã tạo tệp mẫu 'dulieuxettuyendaihoc.csv' thành công!")

# --- Bước 3: Tải dữ liệu ---
file_path = 'dulieuxettuyendaihoc.csv'

if not os.path.exists(file_path):
    print(f"Lỗi: Không tìm thấy tệp '{file_path}'.")
    print("Vui lòng chạy ô mã tạo dữ liệu mẫu trước khi thực hiện bước này.")
else:
    df = pd.read_csv(file_path)

    print("10 dòng đầu tiên của dữ liệu:")
    print(df.head(10))
    print("\n10 dòng cuối cùng của dữ liệu:")
    print(df.tail(10))

    # --- Bước 4: Xử lý dữ liệu thiếu cho cột DT (Dân tộc) ---
    print("\nCác giá trị dân tộc hiện có:", df['DT'].unique())
    df['DT'] = df['DT'].fillna(0)

    # --- Bước 5 & 6: Xử lý dữ liệu thiếu cho các cột điểm số ---
    diem_cols = [
        'T1', 'L1', 'H1', 'S1', 'V1', 'X1', 'D1', 'N1',
        'T2', 'L2', 'H2', 'S2', 'V2', 'X2', 'D2', 'N2',
        'T6', 'L6', 'H6', 'S6', 'V6', 'X6', 'D6', 'N6',
        'DH1', 'DH2', 'DH3'
    ]

    for col in diem_cols:
        if col in df.columns:
            mean_val = df[col].mean()
            df[col] = df[col].fillna(mean_val)

    # --- Bước 7: Tính toán điểm trung bình môn (TBM) ---
    for i, year in enumerate(['1', '2', '6'], 1):
        tbm_col = f'TBM{i}'
        df[tbm_col] = (df[f'T{year}']*2 + df[f'L{year}'] + df[f'H{year}'] + df[f'S{year}'] +
                        df[f'V{year}']*2 + df[f'X{year}'] + df[f'D{year}'] + df[f'N{year}']) / 10

    # --- Bước 8: Xếp loại học lực (XL) ---
    def classify(score):
        if score < 5.0: return 'Y'
        elif score < 6.5: return 'TB'
        elif score < 8.0: return 'K'
        elif score < 9.0: return 'G'
        else: return 'XS'

    for i in range(1, 4):
        df[f'XL{i}'] = df[f'TBM{i}'].apply(classify)

    # --- Bước 9: Quy đổi thang điểm Mỹ (US_TBM - Thang 4) ---
    for i in range(1, 4):
        df[f'US_TBM{i}'] = (df[f'TBM{i}'] / 10) * 4

    # --- Bước 10: Kết quả xét tuyển (KQXT) ---
    def check_admission(row):
        kt = row['KT']
        dh1, dh2, dh3 = row['DH1'], row['DH2'], row['DH3']

        if kt in ['A', 'A1']:
            score = (dh1*2 + dh2 + dh3) / 4
        elif kt == 'B':
            score = (dh1 + dh2*2 + dh3) / 4
        else:
            score = (dh1 + dh2 + dh3) / 3

        return 1 if score >= 5.0 else 0

    df['KQXT'] = df.apply(check_admission, axis=1)

    # --- Bước 11: Lưu trữ file sạch ---
    df.to_csv('processed_dulieuxettuyendaihoc.csv', index=False)
    print("\n--- Xử lý hoàn tất! File 'processed_dulieuxettuyendaihoc.csv' đã được tạo. ---")
