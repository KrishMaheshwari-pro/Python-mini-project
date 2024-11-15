import pandas as pd
import qrcode
from docx import Document
from docx.shared import Inches
import os

def generate_qr_codes(excel_file, data_column, name_column, output_word_file="QR_Codes.docx"):
    df = pd.read_excel(excel_file)
    doc = Document()
    doc.add_heading('Generated QR Codes', level=1)
    qr_dir = "QR_Codes"
    os.makedirs(qr_dir, exist_ok=True)

    for index, row in df.iterrows():   
        qr_data = f"Name: {row[name_column]}, Registration: {row[data_column]}"
        qr_name = str(row[name_column])  
        qr = qrcode.make(qr_data)
        qr_file_path = os.path.join(qr_dir, f"{qr_name}.png")
        qr.save(qr_file_path)

        doc.add_paragraph(f"Name: {qr_name}")
        doc.add_paragraph(f"Data: {qr_data}")
        doc.add_picture(qr_file_path, width=Inches(1.5))
        doc.add_paragraph("\n")

    doc.save(output_word_file)
    print(f"QR codes and data saved to {output_word_file}")

excel_file = 'haha.xlsx'
data_column = 'Registration'
name_column = 'Name'
output_word_file = 'Generated_QR_Codes.docx'

generate_qr_codes(excel_file, data_column, name_column, output_word_file)

generate_qr_codes(excel_file, data_column, name_column, output_word_file)
