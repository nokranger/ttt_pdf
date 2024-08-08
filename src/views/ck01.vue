<template>
  <div>
    <b-container>
      <div style="border: 2px solid gray;border-radius: 10px;height: 450px;box-shadow: 5px 5px 5px #888888;">
        <b-row style="margin: 20px;">
          <b-col>
            <div style="text-align: center;">
              <b-button variant="outline-primary" @click="generatePDF"
                style="box-shadow: 5px 5px 5px #888888;">Preview <b-icon-file-earmark-pdf-fill
                  variant="danger"></b-icon-file-earmark-pdf-fill></b-button>
            </div>
          </b-col>
          <b-col>
          </b-col>
        </b-row>
      </div>
    </b-container>
  </div>
</template>

<script>
import * as XLSX from 'xlsx';
import axios from 'axios';
import { PDFDocument, StandardFonts, rgb } from 'pdf-lib';
import fontkit from '@pdf-lib/fontkit';
import moment from 'moment';
export default {
  data() {
    return {
    }
  },
  methods: {
    async generatePDF(datas) {
      let formatter = new Intl.NumberFormat('en-US', {
        style: 'decimal',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      });
      const pdfDoc = await PDFDocument.create()
      pdfDoc.registerFontkit(fontkit)
      let urls = 'https://script-app.github.io/font/THSarabunNew.ttf'
      let thaiFontBytes = await fetch(urls).then(res => res.arrayBuffer());
      let thaiFont = await pdfDoc.embedFont(thaiFontBytes)
      const imageUrl = 'https://i.imgur.com/LPB21O6.png';
      const imageBytes = await fetch(imageUrl).then((res) => res.arrayBuffer());

      // Embed the image
      const image = await pdfDoc.embedPng(imageBytes);
      const imageDims = image.scale(0.5); // Scale the image if needed
      let page = pdfDoc.addPage();
      // Customize the PDF content based on your requirements
      let xPosition = 50; // Initial x-position for text
      // const yPosition = 700; // Fixed y-position for horizontal alignment
      let yStart = 700; // Fixed y-position for horizontal titles
      let { width, height } = page.getSize();
      const margin = 40;
      height = 730
      let yPosition = height - margin;
      this.titleattach7 = 'fasfasfasfasf'
      const fontSize = 14; //
      const textWidth = thaiFont.widthOfTextAtSize(this.titleattach7, 17);
      const textHeight = thaiFont.heightAtSize(17);
      page.drawImage(image, {
        x: 100, // X-coordinate
        y: 760, // Y-coordinate
        width: imageDims.width,
        height: imageDims.height,
      });
      page.drawText(`บริษัท โตโยต้า ทรานสปอร์ต (ประเทศไทย) จํากัด`, { x: 200, y: 800, size: 20, font: thaiFont });
      page.drawText(`TOYOTA TRANSPORT (THAILAND) CO.,LTD.`, { x: 220, y: 780, size: 18, font: thaiFont });
      page.drawText(`สำนักงานใหญ่`, { x: 130, y: 760, size: 14, font: thaiFont });
      page.drawText(`Head Office`, { x: 130, y: 740, size: 14, font: thaiFont });
      page.drawText(`11/3 หมู่ที่ 1 ต.แสนภูดาษ อ.บ้านโพธิ์ จ.ฉะเชิงเทรา 24140`, { x: 230, y: 760, size: 14, font: thaiFont });
      page.drawText(`11/3 Moo 1 T.Sanphudas, A.Banpho, Chachoengsao 24140`, { x: 230, y: 740, size: 14, font: thaiFont });
      page.drawText(`Tel. 038-578-125-8 Fax. 038-578-129, 038-578-231`, { x: 230, y: 720, size: 14, font: thaiFont });
      page.drawText(`เลขประจำตัวผู้เสียภาษี 0115536007768                                                                                              เลขที่           119/66`, { x: 50, y: 700, size: 14, font: thaiFont });
      page.drawText(`Tax Registration                                                                                                                            No.`, { x: 50, y: 680, size: 14, font: thaiFont });
      page.drawText(`ใบแจ้งหนี้`, { x: 270, y: 660, size: 16, font: thaiFont });
      page.drawText(`INVOICE`, { x: 270, y: 640, size: 16, font: thaiFont });

      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 630, size: 14, font: thaiFont });
      page.drawText(`รหัสลูกค้า O0164`, { x: 50, y: 620, size: 14, font: thaiFont });
      page.drawText(`Customer Code`, { x: 50, y: 600, size: 14, font: thaiFont });
      page.drawText(`ชื่อ TOYOTA LAOS CO., LTD. `, { x: 50, y: 580, size: 14, font: thaiFont });
      page.drawText(`Name`, { x: 50, y: 560, size: 14, font: thaiFont });
      page.drawText(`ที่อยู่ 9 Floor, Royal Square Office Building, no.20 Samsenthai Road,`, { x: 50, y: 540, size: 14, font: thaiFont });
      page.drawText(`Address Nongduong nue Village, Sikottabong District, Vientiane Capital,Lao PDR`, { x: 50, y: 520, size: 14, font: thaiFont });
      page.drawText(`เลขประจำตัวผู้เสียภาษี               สาขา `, { x: 50, y: 500, size: 14, font: thaiFont });
      page.drawText(`Tax Registration No. Branch`, { x: 50, y: 480, size: 14, font: thaiFont });

      page.drawText(`วันที่`, { x: 310, y: 620, size: 14, font: thaiFont });
      page.drawText(`1 JUN 2023`, { x: 410, y: 620, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      page.drawText(`Date`, { x: 310, y: 600, size: 14, font: thaiFont });
      page.drawText(`เงื่อนไขการชำระเงิน`, { x: 310, y: 580, size: 14, font: thaiFont });
      page.drawText(`30 Day`, { x: 430, y: 580, size: 14, font: thaiFont });
      page.drawText(`Terms`, { x: 310, y: 560, size: 14, font: thaiFont });
      page.drawText(`วันครบกำหนดชำระเงิน`, { x: 310, y: 540, size: 14, font: thaiFont });
      page.drawText(`1 JUN 2023`, { x: 430, y: 540, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      page.drawText(`Due Date`, { x: 310, y: 520, size: 14, font: thaiFont });

      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 470, size: 14, font: thaiFont });

      page.drawText(`รายการ`, { x: 150, y: 460, size: 14, font: thaiFont });
      page.drawText(`Descriptions`, { x: 150, y: 450, size: 14, font: thaiFont });
      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 440, size: 14, font: thaiFont });
      page.drawText(`EXPORT TO LAOS TRANSPORTATION FEE`, { x: 50, y: 430, size: 14, font: thaiFont });
      // page.drawText(`ค่าขนส่งรถยนต์ (1 พฤษภาคม - 15 พฤษภาคม 2566)`, { x: 50, y: 410, size: 14, font: thaiFont });
      page.drawText(`(16 - 31 MAY 2023)`, { x: 100, y: 370, size: 14, font: thaiFont });
      page.drawText(`ผิด ตก ยกเว้น E& O.E.`, { x: 150, y: 330, size: 14, font: thaiFont });

      page.drawText(`จำนวน`, { x: 320, y: 460, size: 14, font: thaiFont });
      page.drawText(`QTY`, { x: 320, y: 450, size: 14, font: thaiFont });
      page.drawText(`247`, { x: 320, y: 430, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      // page.drawText(`34`, { x: 320, y: 410, size: 14, font: thaiFont, color: rgb(0, 0, 1) });

      page.drawText(`ราคาหน่วยละ`, { x: 400, y: 460, size: 14, font: thaiFont });
      page.drawText(`Unit Price`, { x: 400, y: 450, size: 14, font: thaiFont });

      page.drawText(`จำนวนเงิน`, { x: 480, y: 460, size: 14, font: thaiFont });
      page.drawText(`Amount`, { x: 480, y: 450, size: 14, font: thaiFont });
      page.drawText(`265,152.00`, { x: 480, y: 430, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      // page.drawText(`168,719.67`, { x: 480, y: 410, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 360, size: 14, font: thaiFont });
      page.drawText(`จำนวนเงินรวม (Total)`, { x: 250, y: 310, size: 14, font: thaiFont });
      page.drawText(`265,152.00`, {  x: 470, y: 310, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 340, size: 14, font: thaiFont });

      page.drawText(`รวม สองแสนหกหมื่นห้าพันหนึ่งร้อยห้าสิบสองบาทถ้วน`, {  x: 50, y: 290, size: 14, font: thaiFont });
      page.drawText(`Bath`, {  x: 50, y: 270, size: 14, font: thaiFont });
      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 310, size: 14, font: thaiFont });

      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 305, size: 14, font: thaiFont });
      page.drawText(`ได้รับใบแจ้งหนี้ไว้เรียบร้อยแล้ว`, {  x: 100, y: 240, size: 14, font: thaiFont });
      page.drawText(`Received the invoice`, {  x: 100, y: 220, size: 14, font: thaiFont });
      page.drawText(`_______________________________________`, {  x: 80, y: 160, size: 14, font: thaiFont });
      page.drawText(`ผู้รับใบแจ้งหนี้ Accepted by`, {  x: 120, y: 140, size: 14, font: thaiFont });
      page.drawText(`วันที่ ___________________________`, {  x: 80, y: 120, size: 14, font: thaiFont });
      page.drawText(`Date`, {  x: 80, y: 100, size: 14, font: thaiFont });

      page.drawText(`ในนาม บริษัท โตโยต้า ทรานสปอร์ต (ประเทศไทย) จำกัด`, {  x: 320, y: 240, size: 14, font: thaiFont });
      page.drawText(`For Toyota Transport (Thailand) Co.,Ltd.`, {  x: 340, y: 220, size: 14, font: thaiFont });
      page.drawText(`_______________________________________`, {  x: 340, y: 160, size: 14, font: thaiFont });
      page.drawText(`ผู้มีอำนาจลงนาม`, {  x: 400, y: 140, size: 14, font: thaiFont });
      page.drawText(`Authorized Signature`, {  x: 400, y: 120, size: 14, font: thaiFont });
      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 200, size: 14, font: thaiFont });

      

//horizental
      page.drawLine({
        start: {x: 40, y: 630}, // X, Y coordinates of the starting point
        end: {x: 570, y: 630}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page.drawLine({
        start: {x: 40, y: 470}, // X, Y coordinates of the starting point
        end: {x: 570, y: 470}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page.drawLine({
        start: {x: 40, y: 440}, // X, Y coordinates of the starting point
        end: {x: 570, y: 440}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page.drawLine({
        start: {x: 40, y: 320}, // X, Y coordinates of the starting point
        end: {x: 570, y: 320}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page.drawLine({
        start: {x: 40, y: 300}, // X, Y coordinates of the starting point
        end: {x: 570, y: 300}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page.drawLine({
        start: {x: 40, y: 265}, // X, Y coordinates of the starting point
        end: {x: 570, y: 265}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page.drawLine({
        start: {x: 40, y: 260}, // X, Y coordinates of the starting point
        end: {x: 570, y: 260}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page.drawLine({
        start: {x: 40, y: 90}, // X, Y coordinates of the starting point
        end: {x: 570, y: 90}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

//vertical
      page.drawLine({
        start: {x: 40, y: 630}, // X, Y coordinates of the starting point
        end: {x: 40, y: 265}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page.drawLine({
        start: {x: 570, y: 630}, // X, Y coordinates of the starting point
        end: {x: 570, y: 265}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page.drawLine({
        start: {x: 300, y: 630}, // X, Y coordinates of the starting point
        end: {x: 300, y: 320}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      page.drawLine({
        start: {x: 380, y: 470}, // X, Y coordinates of the starting point
        end: {x: 380, y: 320}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page.drawLine({
        start: {x: 460, y: 470}, // X, Y coordinates of the starting point
        end: {x: 460, y: 320}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      page.drawLine({
        start: {x: 300, y: 90}, // X, Y coordinates of the starting point
        end: {x: 300, y: 260}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      page.drawLine({
        start: {x: 40, y: 90}, // X, Y coordinates of the starting point
        end: {x: 40, y: 260}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      page.drawLine({
        start: {x: 570, y: 90}, // X, Y coordinates of the starting point
        end: {x: 570, y: 260}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      let page2 = pdfDoc.addPage();
      // page2.drawImage(image, {
      //   x: 100, // X-coordinate
      //   y: 760, // Y-coordinate
      //   width: imageDims.width,
      //   height: imageDims.height,
      // });

      page2.drawText(`เลขทะเบียนการค้าและประจำตัวผู้เสียภาษี Registration No.015536007768 / QF-AC-054 Rev.0`, { x: 200, y: 800, size: 14, font: thaiFont });
      page2.drawText(`ใบส่งสินค้า (Delivery Note)`, { x: 40, y: 770, size: 14, font: thaiFont });
      page2.drawText(`บริษัทโตโยต้า ทรานสปอร์ต (ประเทศไทย) จำกัด`, { x: 360, y: 770, size: 14, font: thaiFont });
      
      page2.drawText(`วันที่ 13-May-2023`, { x: 40, y: 750, size: 14, font: thaiFont });
      page2.drawText(`11/3 หมู่ที่ 1 ต.แสนภูดาษ อำเภอบ้านโพธิ์ จังหวังฉะเชิงเทรา 24140`, { x: 340, y: 750, size: 14, font: thaiFont });

      page2.drawText(`เลขที่ DN B230501664`, { x: 40, y: 730, size: 14, font: thaiFont });
      page2.drawText(`โทร. 038-578125-8 แฟกซ์ 038-578129, 038-578231`, { x: 360, y: 730, size: 14, font: thaiFont });

      page2.drawText(`รหัสลูกค้า O0164`, { x: 40, y: 710, size: 14, font: thaiFont });
      page2.drawText(`ชื่อ พขร. ชื่อ`, { x: 360, y: 710, size: 14, font: thaiFont });

      page2.drawText(`ชื่อตัวแทนจำหน่าย TOYOTA LAOS CO.,LTD.`, { x: 40, y: 690, size: 14, font: thaiFont });
      page2.drawText(`ชื่อ`, { x: 360, y: 690, size: 14, font: thaiFont });

      page2.drawText(`ที่อยู่ `, { x: 40, y: 670, size: 14, font: thaiFont });
      page2.drawText(`ที่อยู่ `, { x: 40, y: 650, size: 14, font: thaiFont });

      page2.drawText(`จาก BONDED WAREHOUSE `, { x: 40, y: 630, size: 14, font: thaiFont });
      page2.drawText(`หมายเลขรถเทรลเลอร์ `, { x: 360, y: 630, size: 14, font: thaiFont });

      page2.drawText(`ถึง VIENTIANE (TLAO SOKYAI STOCK YARD)`, { x: 40, y: 610, size: 14, font: thaiFont });
      page2.drawText(`เวลารถออก`, { x: 360, y: 610, size: 14, font: thaiFont });
      
      page2.drawText(`No.`, { x: 50, y: 590, size: 14, font: thaiFont });
      page2.drawText(`แบบรถ`, { x: 100, y: 590, size: 14, font: thaiFont });
      page2.drawText(`หมายเลขเครื่อง`, { x: 200, y: 590, size: 14, font: thaiFont });
      page2.drawText(`หมายเลขตัวถัง`, { x: 300, y: 590, size: 14, font: thaiFont });
      page2.drawText(`สี`, { x: 400, y: 590, size: 14, font: thaiFont });
      page2.drawText(`หมายเหตุ`, { x: 500, y: 590, size: 14, font: thaiFont });


      page2.drawText(`GDH322L-EDTNY`, { x: 80, y: 570, size: 14, font: thaiFont });
      page2.drawText(`1GD 9139803`, { x: 180, y: 570, size: 14, font: thaiFont });
      page2.drawText(`JTFEB6CPX06044944`, { x: 280, y: 570, size: 14, font: thaiFont });

      page2.drawText(`GDH322L-EDTNY`, { x: 80, y: 550, size: 14, font: thaiFont });
      page2.drawText(`1GD 9141683`, { x: 180, y: 550, size: 14, font: thaiFont });
      page2.drawText(`JTFEB6CP060604538`, { x: 280, y: 550, size: 14, font: thaiFont });

      page2.drawText(`ข้าพเจ้าได้รับสินค้าดังกล่าวข้างต้นในสภาพเรียบร้อย`, { x: 50, y: 430, size: 14, font: thaiFont });

      page2.drawText(`....................................`, { x: 50, y: 420, size: 14, font: thaiFont });
      page2.drawText(`....................................`, { x: 450, y: 420, size: 14, font: thaiFont });

      page2.drawText(`ลายมือชื่อผู้รับของ (ชื่อตัวบรรจง)`, { x: 50, y: 400, size: 14, font: thaiFont });
      page2.drawText(`วันที่     เวลา`, { x: 50, y: 385, size: 14, font: thaiFont });
      page2.drawText(`เอกสารจัดทำโดย`, { x: 450, y: 400, size: 14, font: thaiFont });

//horizental
      page2.drawLine({
        start: {x: 40, y: 790}, // X, Y coordinates of the starting point
        end: {x: 570, y: 790}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 40, y: 720}, // X, Y coordinates of the starting point
        end: {x: 570, y: 720}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 40, y: 600}, // X, Y coordinates of the starting point
        end: {x: 570, y: 600}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 40, y: 580}, // X, Y coordinates of the starting point
        end: {x: 570, y: 580}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 40, y: 450}, // X, Y coordinates of the starting point
        end: {x: 570, y: 450}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 40, y: 380}, // X, Y coordinates of the starting point
        end: {x: 570, y: 380}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
//vertical
      page2.drawLine({
        start: {x: 350, y: 720}, // X, Y coordinates of the starting point
        end: {x: 350, y: 600}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 40, y: 790}, // X, Y coordinates of the starting point
        end: {x: 40, y: 380}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 570, y: 790}, // X, Y coordinates of the starting point
        end: {x: 570, y: 380}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });


      page2.drawLine({
        start: {x: 70, y: 600}, // X, Y coordinates of the starting point
        end: {x: 70, y: 450}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 150, y: 600}, // X, Y coordinates of the starting point
        end: {x: 150, y: 450}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 270, y: 600}, // X, Y coordinates of the starting point
        end: {x: 270, y: 450}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 370, y: 600}, // X, Y coordinates of the starting point
        end: {x: 370, y: 450}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 450, y: 600}, // X, Y coordinates of the starting point
        end: {x: 450, y: 450}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });



      //page 2
      let page3 = pdfDoc.addPage();
      page3.drawImage(image, {
        x: 100, // X-coordinate
        y: 760, // Y-coordinate
        width: imageDims.width,
        height: imageDims.height,
      });
      // page3.drawText(`Page 2`, { x: 20, y: 800, size: 14, font: thaiFont });
      page3.drawText(`บริษัท โตโยต้า ทรานสปอร์ต (ประเทศไทย) จำกัด`, { x: 200, y: 800, size: 14, font: thaiFont });
      page3.drawText(`เลขที่ 11/3 หมู่ที่ 1 ต.แสนภูดาษ อ.บ้านโพธิ์ จ.ฉะเชิงเทรา 24140`, { x: 200, y: 780, size: 14, font: thaiFont });
      page3.drawText(`Tel. 038-578-125-8 Fax. 038-578-129 , 038 578-231`, { x: 200, y: 760, size: 14, font: thaiFont });
      page3.drawText(`ใบแจ้งการใช้บริการขนส่งด้วยรถเทรลเลอร์`, { x: 200, y: 740, size: 14, font: thaiFont });

      page3.drawText(`วันที่`, { x: 400, y: 720, size: 14, font: thaiFont });
      page3.drawText(`10`, { x: 400, y: 700, size: 14, font: thaiFont });

      page3.drawText(`เดือน`, { x: 450, y: 720, size: 14, font: thaiFont });
      page3.drawText(`พฤษภาคม`, { x: 450, y: 700, size: 14, font: thaiFont });

      page3.drawText(`ปี`, { x: 540, y: 720, size: 14, font: thaiFont });
      page3.drawText(`2566`, { x: 540, y: 700, size: 14, font: thaiFont });

      page3.drawText(`ตัวแทนจำหน่ายผู้ขอใช้บริการ (DEALER)`, { x: 20, y: 680, size: 14, font: thaiFont });
      page3.drawText(`โตโยฟูจิ โลจิสติกส์ ประเทศไทย TFLT`, { x: 200, y: 680, size: 14, font: thaiFont });
      page3.drawText(`ขอใช้บริการขนส่งบรรทุกรถยนต์จำนวน`, { x: 380, y: 680, size: 14, font: thaiFont });
      page3.drawText(`7 คัน`, { x: 540, y: 680, size: 14, font: thaiFont });

      page3.drawText(`ลำดับ`, { x: 20, y: 640, size: 14, font: thaiFont });
      page3.drawText(`แบบรถ`, { x:100, y: 640, size: 14, font: thaiFont });
      page3.drawText(`Invoice No.`, { x: 190, y: 640, size: 14, font: thaiFont });
      page3.drawText(`QTY`, { x: 240, y: 640, size: 14, font: thaiFont });
      page3.drawText(`สี (Dealer)`, { x: 270, y: 640, size: 14, font: thaiFont });
      page3.drawText(`ทะเบียนรถ`, { x: 330, y: 640, size: 14, font: thaiFont });
      page3.drawText(`รายละเอียดการขนส่ง`, { x: 400, y: 660, size: 14, font: thaiFont });
      page3.drawText(`จาก`, { x: 390, y: 640, size: 14, font: thaiFont });
      page3.drawText(`ถึง`, { x: 530, y: 640, size: 14, font: thaiFont });
      page3.drawText(`สถานที่`, { x: 400, y: 620, size: 14, font: thaiFont });
      page3.drawText(`วันที่`, { x: 450, y: 620, size: 14, font: thaiFont });
      page3.drawText(`สถานที่`, { x: 500, y: 620, size: 14, font: thaiFont });


      page3.drawText(`1`, { x: 30, y: 600, size: 14, font: thaiFont });
      page3.drawText(`2`, { x: 30, y: 560, size: 14, font: thaiFont });
      page3.drawText(`3`, { x: 30, y: 520, size: 14, font: thaiFont });
      page3.drawText(`4`, { x: 30, y: 480, size: 14, font: thaiFont });
      page3.drawText(`5`, { x: 30, y: 440, size: 14, font: thaiFont });
      page3.drawText(`6`, { x: 30, y: 420, size: 14, font: thaiFont });
      page3.drawText(`7`, { x: 30, y: 400, size: 14, font: thaiFont });

      page3.drawText(`TOYOTA HIACE 14 SEATERS`, { x: 50, y: 600, size: 14, font: thaiFont });
      page3.drawText(`TOYOTA HIACE 14 SEATERS`, { x: 50, y: 580, size: 14, font: thaiFont });

      page3.drawText(`TOYOTA HIACE 14 SEATERS`, { x: 50, y: 560, size: 14, font: thaiFont });
      page3.drawText(`TOYOTA HIACE 14 SEATERS`, { x: 50, y: 540, size: 14, font: thaiFont });

      page3.drawText(`TOYOTA HIACE 14 SEATERS`, { x: 50, y: 520, size: 14, font: thaiFont });
      page3.drawText(`TOYOTA HIACE 14 SEATERS`, { x: 50, y: 500, size: 14, font: thaiFont });

      page3.drawText(`TOYOTA HIACE 14 SEATERS`, { x: 50, y: 480, size: 14, font: thaiFont });
      page3.drawText(`TOYOTA HIACE 14 SEATERS`, { x: 50, y: 460, size: 14, font: thaiFont });

      page3.drawText(`TOYOTA LAND CRUISER PRADO`, { x: 50, y: 440, size: 14, font: thaiFont });
      page3.drawText(`TOYOTA HIACE 14 SEATERS`, { x: 50, y: 420, size: 14, font: thaiFont });
      page3.drawText(`TOYOTA LAND CRUISER PRADO`, { x: 50, y: 400, size: 14, font: thaiFont });

      page3.drawText(`B23040080`, { x: 190, y: 600, size: 14, font: thaiFont });
      page3.drawText(`B23040080`, { x: 190, y: 580, size: 14, font: thaiFont });

      page3.drawText(`B23040081`, { x: 190, y: 560, size: 14, font: thaiFont });
      page3.drawText(`B23040081`, { x: 190, y: 540, size: 14, font: thaiFont });

      page3.drawText(`B23040082`, { x: 190, y: 520, size: 14, font: thaiFont });
      page3.drawText(`B23040082`, { x: 190, y: 500, size: 14, font: thaiFont });

      page3.drawText(`B23040083`, { x: 190, y: 480, size: 14, font: thaiFont });
      page3.drawText(`B23040083`, { x: 190, y: 460, size: 14, font: thaiFont });

      page3.drawText(`B23040084`, { x: 190, y: 440, size: 14, font: thaiFont });
      page3.drawText(`B23040087`, { x: 190, y: 420, size: 14, font: thaiFont });
      page3.drawText(`B23040064`, { x: 190, y: 400, size: 14, font: thaiFont });

      page3.drawText(`1`, { x: 250, y: 600, size: 14, font: thaiFont });
      page3.drawText(`1`, { x: 250, y: 580, size: 14, font: thaiFont });
      page3.drawText(`1`, { x: 250, y: 560, size: 14, font: thaiFont });
      page3.drawText(`1`, { x: 250, y: 540, size: 14, font: thaiFont });
      page3.drawText(`1`, { x: 250, y: 520, size: 14, font: thaiFont });
      page3.drawText(`1`, { x: 250, y: 500, size: 14, font: thaiFont });
      page3.drawText(`1`, { x: 250, y: 480, size: 14, font: thaiFont });
      page3.drawText(`1`, { x: 250, y: 460, size: 14, font: thaiFont });
      page3.drawText(`1`, { x: 250, y: 440, size: 14, font: thaiFont });
      page3.drawText(`1`, { x: 250, y: 420, size: 14, font: thaiFont });
      page3.drawText(`1`, { x: 250, y: 400, size: 14, font: thaiFont });

      page3.drawText(`Flatbed`, { x: 270, y: 600, size: 14, font: thaiFont });
      page3.drawText(`Flatbed`, { x: 270, y: 580, size: 14, font: thaiFont });
      page3.drawText(`Flatbed`, { x: 270, y: 560, size: 14, font: thaiFont });
      page3.drawText(`Flatbed`, { x: 270, y: 540, size: 14, font: thaiFont });
      page3.drawText(`Flatbed`, { x: 270, y: 520, size: 14, font: thaiFont });
      page3.drawText(`Flatbed`, { x: 270, y: 500, size: 14, font: thaiFont });
      page3.drawText(`Flatbed`, { x: 270, y: 480, size: 14, font: thaiFont });
      page3.drawText(`Flatbed`, { x: 270, y: 460, size: 14, font: thaiFont });
      page3.drawText(`Slide-on `, { x: 270, y: 440, size: 14, font: thaiFont });
      page3.drawText(`Slide-on `, { x: 270, y: 420, size: 14, font: thaiFont });
      page3.drawText(`Slide-on `, { x: 270, y: 400, size: 14, font: thaiFont });

      page3.drawText(`Trailer no.1`, { x: 330, y: 600, size: 14, font: thaiFont });
      page3.drawText(`Trailer no.1`, { x: 330, y: 580, size: 14, font: thaiFont });

      page3.drawText(`Trailer no.2`, { x: 330, y: 560, size: 14, font: thaiFont });
      page3.drawText(`Trailer no.2`, { x: 330, y: 540, size: 14, font: thaiFont });

      page3.drawText(`Trailer no.3`, { x: 330, y: 520, size: 14, font: thaiFont });
      page3.drawText(`Trailer no.3`, { x: 330, y: 500, size: 14, font: thaiFont });

      page3.drawText(`Trailer no.4`, { x: 330, y: 480, size: 14, font: thaiFont });
      page3.drawText(`Trailer no.4`, { x: 330, y: 460, size: 14, font: thaiFont });

      page3.drawText(`Trailer no.5`, { x: 330, y: 440, size: 14, font: thaiFont });
      page3.drawText(`Trailer no.6`, { x: 330, y: 420, size: 14, font: thaiFont });
      page3.drawText(`Trailer no.7`, { x: 330, y: 400, size: 14, font: thaiFont });

      page3.drawText(`16-May-23`, { x: 370, y: 500, size: 14, font: thaiFont });
      page3.drawText(`A1 termina`, { x: 420, y: 500, size: 14, font: thaiFont });
      page3.drawText(`17-May-23`, { x: 480, y: 500, size: 14, font: thaiFont });
      page3.drawText(`หนองคาย - ท่านาแล้ง`, { x: 520, y: 500, size: 14, font: thaiFont });

      page3.drawText(`16-May-23`, { x: 370, y: 420, size: 14, font: thaiFont });
      page3.drawText(`A1 termina`, { x: 420, y: 420, size: 14, font: thaiFont });
      page3.drawText(`17-May-23`, { x: 480, y: 420, size: 14, font: thaiFont });
      page3.drawText(`ช้องเม็ก - ปากเซ`, { x: 520, y: 420, size: 14, font: thaiFont });

      page3.drawText(`16-May-23`, { x: 370, y: 400, size: 14, font: thaiFont });
      page3.drawText(`A5 termina`, { x: 420, y: 400, size: 14, font: thaiFont });
      page3.drawText(`17-May-23`, { x: 480, y: 400, size: 14, font: thaiFont });
      page3.drawText(`มุกดาหาร - สะหวันนะเขต`, { x: 520, y: 400, size: 14, font: thaiFont });

      page3.drawText(`( เริ่มงาน 0800 hrs )`, { x: 400, y: 380, size: 14, font: thaiFont });

      page3.drawText(`GRAND TOTAL`, { x: 50, y: 360, size: 14, font: thaiFont });
      page3.drawText(`11`, { x: 250, y: 360, size: 14, font: thaiFont });

      page3.drawText(`หมายเหตุ: ค่าใช้จ่ายรับผิดชอบโดย`, { x: 20, y: 320, size: 12, font: thaiFont });

      page3.drawText(`1. ผู้ขอใช้บริการ (ตัวบรรจง) จริยา, อรุณ, ศุภกฤต`, { x: 50, y: 300, size: 12, font: thaiFont });
      page3.drawText(`2. บริษัท`, { x: 50, y: 280, size: 12, font: thaiFont });
      page3.drawText(`3. Budget ของ`, { x: 50, y: 260, size: 12, font: thaiFont });
      page3.drawText(`4. ผู้จัดการแผนก / ฝ่าย`, { x: 50, y: 240, size: 12, font: thaiFont });
      page3.drawText(`* กรุณากรอกรายละเอียดให้ครบถ้วน`, { x: 50, y: 220, size: 12, font: thaiFont });

      page3.drawText(`โทร 02 632 8868 / 081 636 4427`, { x: 200, y: 280, size: 12, font: thaiFont });
      page3.drawText(`โตโยฟูจิ โลจิสติกส์ (ประเทศไทย) จำกัด`, { x: 200, y: 260, size: 12, font: thaiFont });
      page3.drawText(`__________________________________`, { x: 200, y: 240, size: 12, font: thaiFont });
      page3.drawText(`ชื่อ ลูกค้าที่ออกในใบเสร็จ`, { x: 200, y: 220, size: 12, font: thaiFont });

      page3.drawText(`กรุณามารับรถเอง`, { x: 400, y: 320, size: 12, font: thaiFont });

      page3.drawText(`ชื่อผู้ที่มารับรถเอง`, { x: 450, y: 300, size: 12, font: thaiFont });
      page3.drawText(`มาจากบริษัท`, { x: 450, y: 280, size: 12, font: thaiFont });

      page3.drawText(`TOYOFUJI SHIPPING SINGAPORE PTE LTD. AGENT ACTED BY FUJITRANS (THAILAND) CO., LTD`, { x: 300, y: 220, size: 12, font: thaiFont });
      page3.drawText(`944 MITRTOWN OFFICE TOWER, 16th FLOOR, UNIT. 1607 RAMA 4 ROAD, WANGMAI, PATHUMWAN, BANGKOK 10330, THAILAND`, { x: 300, y: 200, size: 12, font: thaiFont });
      page3.drawText(`LOCATION: HEAD OFFICE TAX ID: 0993000009266`, { x: 300, y: 180, size: 12, font: thaiFont });

      //horizental
      page3.drawLine({
      start: {x: 380, y: 730}, // X, Y coordinates of the starting point
      end: {x: 570, y: 730}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 380, y: 710}, // X, Y coordinates of the starting point
      end: {x: 570, y: 710}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 690}, // X, Y coordinates of the starting point
      end: {x: 570, y: 690}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 670}, // X, Y coordinates of the starting point
      end: {x: 570, y: 670}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 610}, // X, Y coordinates of the starting point
      end: {x: 570, y: 610}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });


      page3.drawLine({
      start: {x: 20, y: 590}, // X, Y coordinates of the starting point
      end: {x: 380, y: 590}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 570}, // X, Y coordinates of the starting point
      end: {x: 380, y: 570}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 550}, // X, Y coordinates of the starting point
      end: {x: 380, y: 550}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 530}, // X, Y coordinates of the starting point
      end: {x: 380, y: 530}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 510}, // X, Y coordinates of the starting point
      end: {x: 380, y: 510}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 490}, // X, Y coordinates of the starting point
      end: {x: 380, y: 490}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 470}, // X, Y coordinates of the starting point
      end: {x: 380, y: 470}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 450}, // X, Y coordinates of the starting point
      end: {x: 380, y: 450}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });


      page3.drawLine({
      start: {x: 20, y: 430}, // X, Y coordinates of the starting point
      end: {x: 570, y: 430}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 410}, // X, Y coordinates of the starting point
      end: {x: 570, y: 410}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 390}, // X, Y coordinates of the starting point
      end: {x: 570, y: 390}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 370}, // X, Y coordinates of the starting point
      end: {x: 570, y: 370}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 350}, // X, Y coordinates of the starting point
      end: {x: 570, y: 350}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });


      page3.drawLine({
      start: {x: 20, y: 330}, // X, Y coordinates of the starting point
      end: {x: 570, y: 330}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 20, y: 170}, // X, Y coordinates of the starting point
      end: {x: 570, y: 170}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });


      page3.drawLine({
      start: {x: 380, y: 650}, // X, Y coordinates of the starting point
      end: {x: 570, y: 650}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
      start: {x: 380, y: 630}, // X, Y coordinates of the starting point
      end: {x: 570, y: 630}, // X, Y coordinates of the ending point
      thickness: 0.5, // Line thickness
      color: rgb(0, 0, 0), // Line color (black)
      });

      //vertical
      page3.drawLine({
        start: {x: 380, y: 730}, // X, Y coordinates of the starting point
        end: {x: 380, y: 670}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
        start: {x: 430, y: 730}, // X, Y coordinates of the starting point
        end: {x: 430, y: 690}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
        start: {x: 530, y: 730}, // X, Y coordinates of the starting point
        end: {x: 530, y: 670}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
        start: {x: 570, y: 730}, // X, Y coordinates of the starting point
        end: {x: 570, y: 670}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });


      page3.drawLine({
        start: {x: 20, y: 690}, // X, Y coordinates of the starting point
        end: {x: 20, y: 170}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
        start: {x: 49, y: 670}, // X, Y coordinates of the starting point
        end: {x: 49, y: 330}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      page3.drawLine({
        start: {x: 190, y: 690}, // X, Y coordinates of the starting point
        end: {x: 190, y: 670}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      page3.drawLine({
        start: {x: 190, y: 670}, // X, Y coordinates of the starting point
        end: {x: 190, y: 330}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
        start: {x: 249, y: 670}, // X, Y coordinates of the starting point
        end: {x: 249, y: 330}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
        start: {x: 269, y: 670}, // X, Y coordinates of the starting point
        end: {x: 269, y: 330}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
        start: {x: 329, y: 670}, // X, Y coordinates of the starting point
        end: {x: 329, y: 330}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
        start: {x: 380, y: 670}, // X, Y coordinates of the starting point
        end: {x: 380, y: 230}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });


      page3.drawLine({
        start: {x: 430, y: 630}, // X, Y coordinates of the starting point
        end: {x: 430, y: 390}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
        start: {x: 480, y: 630}, // X, Y coordinates of the starting point
        end: {x: 480, y: 330}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
        start: {x: 530, y: 630}, // X, Y coordinates of the starting point
        end: {x: 530, y: 330}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page3.drawLine({
        start: {x: 570, y: 670}, // X, Y coordinates of the starting point
        end: {x: 570, y: 170}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });







      const pdfBytes = await pdfDoc.save();
      const blob = new Blob([pdfBytes], { type: 'application/pdf' });
      const url = URL.createObjectURL(blob);
      window.open(url, '_blank');
    },
  }
}
</script>