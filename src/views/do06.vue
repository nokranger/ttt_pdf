<template>
  <div>
    <!-- <nav>
      <router-link to="/dashboard">Import & Export</router-link> ||
      <router-link to="/Attached">Attached</router-link> ||
      <router-link to="/payroll">Payroll</router-link>
    </nav> -->
    <b-container>
      <!-- <div>
        <h1 style="text-shadow: 2px 2px 5px black;">Attached</h1>
      </div> -->
      <div style="border: 2px solid gray;border-radius: 10px;height: 450px;box-shadow: 5px 5px 5px #888888;">
        <!-- <b-row style="margin: 20px;">
          <b-col>
            <div style="font-size: 20px;text-align: left;margin-left: 10px;">ตั้งแต่วันที่ (TNOS & Instructor )</div>
            <b-form-datepicker style="width: 100%;" id="example-datepickerattach7" v-model="dateattach7from"
              class="mb-2"></b-form-datepicker>
          </b-col>
          <b-col>
            <div style="font-size: 20px;text-align: left;margin-left: 10px;">ถึงวันที่ (TNOS & Instructor )</div>
            <b-form-datepicker style="width: 100%;" id="example-datepickerattach72" v-model="dateattach7to"
              class="mb-2"></b-form-datepicker>
          </b-col>
          <b-col>
            <div style="font-size: 20px;text-align: left;margin-left: 10px;">วันที่จ่ายเงิน</div>
            <b-form-datepicker style="width: 100%;" id="example-datepickerattach73" v-model="dateattach7select"
              class="mb-2"></b-form-datepicker>
          </b-col>
        </b-row> -->
        <!-- <b-row style="margin: 20px;">
          <b-col>
            <div style="font-size: 20px;text-align: left;margin-left: 10px;">ตั้งแต่วันที่ (Welfare)</div>
            <b-form-datepicker style="width: 100%;" id="example-datepickerattach7welfare"
              v-model="dateattach7welfareform" class="mb-2"></b-form-datepicker>
          </b-col>
          <b-col>
            <div style="font-size: 20px;text-align: left;margin-left: 10px;">ถึงวันที่ (Welfare)</div>
            <b-form-datepicker style="width: 100%;" id="example-datepickerattach72welfare"
              v-model="dateattach7welfareto" class="mb-2"></b-form-datepicker>
          </b-col>
          <b-col>
          </b-col>
        </b-row> -->
        <!-- <b-row style="margin: 20px;">
          <b-col>
            <div>
              <div style="font-size: 20px;text-align: left;margin-left: 10px;">กรุณากรอกหัวข้อรายงาน</div>
              <b-input placeholder="Enter your Title Report" v-model="titleattach7"></b-input>
            </div>
          </b-col>
          <b-col></b-col>
          <b-col></b-col>
        </b-row> -->
        <b-row style="margin: 20px;">
          <b-col>
            <div style="text-align: center;">
              <b-button variant="outline-primary" @click="generatePDF"
                style="box-shadow: 5px 5px 5px #888888;">Preview <b-icon-file-earmark-pdf-fill
                  variant="danger"></b-icon-file-earmark-pdf-fill></b-button>
            </div>
          </b-col>
          <!-- <b-col>
            <div style="text-align: center;">
              <b-button variant="outline-primary" @click="getAttach7excel"
                style="box-shadow: 5px 5px 5px #888888;">Export <b-icon-file-earmark-excel-fill
                  variant="success"></b-icon-file-earmark-excel-fill></b-button>
            </div>
          </b-col> -->
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
      excelarray: [],
      excelarrayattach7: [],
      excelarrayattach72: [],
      excelarrayattach721: [],
      excelarrayattach731: [],
      excelarrayattach73: [],
      excelarrayattach9: [],
      excelarrayattach92: [],
      excelarrayattach93: [],
      dateattach7from: '',
      dateattach7to: '',
      dateattach7select: '',
      dataattach7one: '',
      dateattach7welfareform: '',
      dateattach7welfareto: '',
      pdfdata: '',
      titleattach7: '',
      selectedattach7: null,
      optionsattach7: [
        { value: null, text: 'ดูทั้งหมด' },
        { value: 1, text: 'จ่ายแล้ว' },
        { value: 2, text: 'ยังไม่จ่าย' }
      ],
      sumValue: 0,
      status: '0',
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
      page.drawText(`เลขประจำตัวผู้เสียภาษี 0115536007768                                                                                              เลขที่           125/6`, { x: 50, y: 700, size: 14, font: thaiFont });
      page.drawText(`Tax Registration                                                                                                                            No.`, { x: 50, y: 680, size: 14, font: thaiFont });
      page.drawText(`ใบแจ้งหนี้`, { x: 270, y: 660, size: 16, font: thaiFont });
      page.drawText(`INVOICE`, { x: 270, y: 640, size: 16, font: thaiFont });

      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 630, size: 14, font: thaiFont });
      page.drawText(`รหัสลูกค้า 39300`, { x: 50, y: 620, size: 14, font: thaiFont });
      page.drawText(`Customer Code`, { x: 50, y: 600, size: 14, font: thaiFont });
      page.drawText(`ชื่อ บริษัท โตโยต้า มอเตอร์ ประเทศไทย จำกัด`, { x: 50, y: 580, size: 14, font: thaiFont });
      page.drawText(`Name`, { x: 50, y: 560, size: 14, font: thaiFont });
      page.drawText(`ที่อยู่ 186/1 หมู่ 1 ถ.ทางรถไฟ ต.สำโรงใต้`, { x: 50, y: 540, size: 14, font: thaiFont });
      page.drawText(`Address อ.พระปะแดง จ.สมุทรปราการ 10130`, { x: 50, y: 520, size: 14, font: thaiFont });
      page.drawText(`เลขประจำตัวผู้เสียภาษี 0105505001679 สาขา สำนักงานใหญ่`, { x: 50, y: 500, size: 14, font: thaiFont });
      page.drawText(`Tax Registration No. Branch`, { x: 50, y: 480, size: 14, font: thaiFont });

      page.drawText(`วันที่`, { x: 310, y: 620, size: 14, font: thaiFont });
      page.drawText(`25 มิถุนายน 2567`, { x: 410, y: 620, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      page.drawText(`Date`, { x: 310, y: 600, size: 14, font: thaiFont });
      page.drawText(`เงื่อนไขการชำระเงิน`, { x: 310, y: 580, size: 14, font: thaiFont });
      page.drawText(`30 วัน`, { x: 430, y: 580, size: 14, font: thaiFont });
      page.drawText(`Terms`, { x: 310, y: 560, size: 14, font: thaiFont });
      page.drawText(`วันครบกำหนดชำระเงิน`, { x: 310, y: 540, size: 14, font: thaiFont });
      page.drawText(`20 กรกฎาคม 2567`, { x: 430, y: 540, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      page.drawText(`Due Date`, { x: 310, y: 520, size: 14, font: thaiFont });

      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 470, size: 14, font: thaiFont });

      page.drawText(`รายการ`, { x: 150, y: 460, size: 14, font: thaiFont });
      page.drawText(`Descriptions`, { x: 150, y: 450, size: 14, font: thaiFont });
      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 440, size: 14, font: thaiFont });
      page.drawText(`ค่าขนส่งรถยนต์ (1 มิถุนายน - 15 มิถุนายน 2567)`, { x: 50, y: 430, size: 14, font: thaiFont });
      // page.drawText(`ค่าขนส่งรถยนต์ (1 พฤษภาคม - 15 พฤษภาคม 2566)`, { x: 50, y: 410, size: 14, font: thaiFont });
      page.drawText(`YARD OUT TO DEALER - FLEXIBLE CLUSTER`, { x: 100, y: 370, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      page.drawText(`ผิด ตก ยกเว้น E& O.E.`, { x: 150, y: 330, size: 14, font: thaiFont });

      page.drawText(`จำนวน`, { x: 320, y: 460, size: 14, font: thaiFont });
      page.drawText(`QTY`, { x: 320, y: 450, size: 14, font: thaiFont });
      page.drawText(`5,024`, { x: 320, y: 430, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      // page.drawText(`34`, { x: 320, y: 410, size: 14, font: thaiFont, color: rgb(0, 0, 1) });

      page.drawText(`ราคาหน่วยละ`, { x: 400, y: 460, size: 14, font: thaiFont });
      page.drawText(`Unit Price`, { x: 400, y: 450, size: 14, font: thaiFont });

      page.drawText(`จำนวนเงิน`, { x: 480, y: 460, size: 14, font: thaiFont });
      page.drawText(`Amount`, { x: 480, y: 450, size: 14, font: thaiFont });
      page.drawText(`2,700,618`, { x: 480, y: 430, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      // page.drawText(`168,719.67`, { x: 480, y: 410, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 360, size: 14, font: thaiFont });
      page.drawText(`จำนวนเงินรวม (Total)`, { x: 250, y: 310, size: 14, font: thaiFont });
      page.drawText(`2,700,618`, {  x: 470, y: 310, size: 14, font: thaiFont, color: rgb(0, 0, 1) });
      // page.drawText(`___________________________________________________________________________________________________________`, { x: 40, y: 340, size: 14, font: thaiFont });

      page.drawText(`รวม สองล้านเจ็ดแสนหกร้อยสิบแปดบาทถ้วน`, {  x: 50, y: 290, size: 14, font: thaiFont });
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
      page2.drawText(`SUMMARY TRANSPORTATION : SC SONGKHLA TO DEALER`, { x: 20, y: 800, size: 14, font: thaiFont });
      page2.drawText(`Payment Month: April'2023 Period: 16-30 April'2023 Price revision: April'2023`, { x: 20, y: 780, size: 14, font: thaiFont });
      page2.drawLine({
        start: {x: 350, y: 800}, // X, Y coordinates of the starting point
        end: {x: 350, y: 700}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 400, y: 780}, // X, Y coordinates of the starting point
        end: {x: 400, y: 700}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 450, y: 800}, // X, Y coordinates of the starting point
        end: {x: 450, y: 700}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      page2.drawLine({
        start: {x: 460, y: 800}, // X, Y coordinates of the starting point
        end: {x: 460, y: 700}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 510, y: 780}, // X, Y coordinates of the starting point
        end: {x: 510, y: 700}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 560, y: 800}, // X, Y coordinates of the starting point
        end: {x: 560, y: 700}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });


      page2.drawLine({
        start: {x: 350, y: 800}, // X, Y coordinates of the starting point
        end: {x: 450, y: 800}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 460, y: 800}, // X, Y coordinates of the starting point
        end: {x: 560, y: 800}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      page2.drawLine({
        start: {x: 350, y: 700}, // X, Y coordinates of the starting point
        end: {x: 450, y: 700}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 460, y: 700}, // X, Y coordinates of the starting point
        end: {x: 560, y: 700}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      page2.drawLine({
        start: {x: 350, y: 780}, // X, Y coordinates of the starting point
        end: {x: 450, y: 780}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 350, y: 760}, // X, Y coordinates of the starting point
        end: {x: 450, y: 760}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 350, y: 720}, // X, Y coordinates of the starting point
        end: {x: 450, y: 720}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      page2.drawLine({
        start: {x: 460, y: 780}, // X, Y coordinates of the starting point
        end: {x: 560, y: 780}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 460, y: 760}, // X, Y coordinates of the starting point
        end: {x: 560, y: 760}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });
      page2.drawLine({
        start: {x: 460, y: 720}, // X, Y coordinates of the starting point
        end: {x: 560, y: 720}, // X, Y coordinates of the ending point
        thickness: 0.5, // Line thickness
        color: rgb(0, 0, 0), // Line color (black)
      });

      page2.drawText(`TMT`, { x: 390, y: 785, size: 20, font: thaiFont });
      page2.drawText(`TTT`, { x: 500, y: 785, size: 20, font: thaiFont });

      page2.drawText(`Approved`, { x: 355, y: 765, size: 14, font: thaiFont });
      page2.drawText(`Checked`, { x: 410, y: 765, size: 14, font: thaiFont });

      page2.drawText(`Checked`, { x: 470, y: 765, size: 14, font: thaiFont });
      page2.drawText(`Issued`, { x: 520, y: 765, size: 14, font: thaiFont });

      page2.drawText(`SU`, { x: 70, y: 680, size: 14, font: thaiFont });
      page2.drawText(`SO`, { x: 90, y: 680, size: 14, font: thaiFont });
      page2.drawText(`SU`, { x: 110, y: 680, size: 14, font: thaiFont });
      page2.drawText(`SO`, { x: 130, y: 680, size: 14, font: thaiFont });

      page2.drawText(`SU`, { x: 180, y: 680, size: 14, font: thaiFont });
      page2.drawText(`SU`, { x: 200, y: 680, size: 14, font: thaiFont });
      page2.drawText(`SU`, { x: 220, y: 680, size: 14, font: thaiFont });
      page2.drawText(`SU`, { x: 240, y: 680, size: 14, font: thaiFont });

      page2.drawText(`SO`, { x: 290, y: 680, size: 14, font: thaiFont });
      page2.drawText(`SO`, { x: 310, y: 680, size: 14, font: thaiFont });
      page2.drawText(`SO`, { x: 330, y: 680, size: 14, font: thaiFont });
      page2.drawText(`SO`, { x: 350, y: 680, size: 14, font: thaiFont });

      page2.drawText(`SU`, { x: 70, y: 660, size: 14, font: thaiFont });
      page2.drawText(`SO`, { x: 90, y: 660, size: 14, font: thaiFont });

      page2.drawText(`A`, { x: 120, y: 660, size: 14, font: thaiFont });

      page2.drawText(`B`, { x: 150, y: 660, size: 14, font: thaiFont });
      page2.drawText(`C`, { x: 170, y: 660, size: 14, font: thaiFont });

      page2.drawText(`D`, { x: 190, y: 660, size: 14, font: thaiFont });
      page2.drawText(`E`, { x: 210, y: 660, size: 14, font: thaiFont });
      page2.drawText(`F`, { x: 230, y: 660, size: 14, font: thaiFont });
      page2.drawText(`G`, { x: 250, y: 660, size: 14, font: thaiFont });
      page2.drawText(`H`, { x: 270, y: 660, size: 14, font: thaiFont });

      page2.drawText(`I=(D+E)`, { x: 280, y: 660, size: 14, font: thaiFont });
      page2.drawText(`I=(F+G+H)`, { x: 320, y: 660, size: 14, font: thaiFont });

      // page2.drawText(`Issued`, { x: 520, y: 765, size: 14, font: thaiFont });
      // page2.drawLine({
      //   start: {x: 40, y: 630}, // X, Y coordinates of the starting point
      //   end: {x: 570, y: 630}, // X, Y coordinates of the ending point
      //   thickness: 0.5, // Line thickness
      //   color: rgb(0, 0, 0), // Line color (black)
      // });
      // page.drawText(`สรุปยอดเงินเบี้ยเลี้ยง/ค่าขับและสวัสดิการของพนักงาน`, { x: 140, y: 780, size: 20, font: thaiFont });
      // page.drawText('fasfasffsfsaf', { x: page.getWidth() / 2.2 - textWidth / 2.2, y: 780, size: 20, font: thaiFont });
      // page.drawText(`เข้าบัญชีพนักงานวันที่ `, { x: 190, y: 760, size: 20, font: thaiFont });
      // page.drawText(`__________________________________________________________________________________`, { x: 10, y: 750, size: 20, font: thaiFont });
      // page.drawText(`ลำดับ`, { x: 50, y: 720, size: fontSize, font: thaiFont });
      // page.drawText(`เลขที่บัญชี`, { x: 100, y: 720, size: fontSize, font: thaiFont });
      // page.drawText(`รหัส`, { x: 220, y: 720, size: fontSize, font: thaiFont });
      // page.drawText(`ชื่อ - นามสกุล`, { x: 300, y: 720, size: fontSize, font: thaiFont });
      // page.drawText(`จำนวนเงิน`, { x: 500, y: 720, size: fontSize, font: thaiFont });
      // page.drawText(`__________________________________________________________________________________`, { x: 10, y: 710, size: 20, font: thaiFont });
      let count = 0
      let count2 = 0
      let count3 = 0
      let countPage = 1
      // // let sumValue = 0
      // // var sumValue = datas.reduce(function(_this, val) {
      // //     return _this + parseInt(val.total_allowance)
      // // }, 0);
      // // let sumValue = datas.reduce((acc, obj) => acc += parseInt(obj.total_allowance), 0);
      // // page.drawText(`Page${countPage}`, { x: 450, y: 720 , size: fontSize});
      // for (const data of datas) {
      //   // console.log('count', data.length)
      //   // if (count === datas.length) {
      //   //   sumValue = data.reduce((acc, obj) => acc += parseInt(obj.total_allowance), 0);
      //   // }
      //   // console.log('countValue', count2)
      //   const sumValue = await datas.reduce((acc, obj) => acc + parseFloat(obj.total_allowance), 0);
      //   // const titleHeight = 20; // Adjust as needed
      //   const descriptionHeight = 15; // Adjust as needed

      //   // Check if there is enough space on the current page
      //   if (yPosition - descriptionHeight < margin) {
      //     countPage++;
      //     // Create a new page if the content doesn't fit
      //     page = pdfDoc.addPage();
      //     page.drawText(`บริษัท โตโยต้า ทรานสปอร์ต (ประเทศไทย) จํากัด`, { x: 170, y: 800, size: 20, font: thaiFont });
      //     // page.drawText(`สรุปยอดเงินเบี้ยเลี้ยง/ค่าขับและสวัสดิการของพนักงาน`, { x: 140, y: 780, size: 20, font: thaiFont });
      //     page.drawText(`${this.titleattach7}`, { x: page.getWidth() / 2.2 - textWidth / 2.2, y: 780, size: 20, font: thaiFont });
      //     page.drawText(`เข้าบัญชีพนักงานวันที่ ${moment(this.dateattach7select).format('L')}`, { x: 190, y: 760, size: 20, font: thaiFont });
      //     page.drawText(`__________________________________________________________________________________`, { x: 10, y: 750, size: 20, font: thaiFont });
      //     page.drawText(`ลำดับ`, { x: 50, y: 720, size: fontSize, font: thaiFont });
      //     page.drawText(`เลขที่บัญชี`, { x: 100, y: 720, size: fontSize, font: thaiFont });
      //     page.drawText(`รหัส`, { x: 220, y: 720, size: fontSize, font: thaiFont });
      //     page.drawText(`ชื่อ - นามสกุล`, { x: 300, y: 720, size: fontSize, font: thaiFont });
      //     page.drawText(`จำนวนเงิน`, { x: 500, y: 720, size: fontSize, font: thaiFont });
      //     page.drawText(`__________________________________________________________________________________`, { x: 10, y: 710, size: 20, font: thaiFont });
      //     // page.drawText(`Page${countPage}`, { x: 450, y: 720 , size: fontSize});
      //     yPosition = height - margin;
      //   }
      //   page.drawText(`${count + 1}`, { x: 50, y: yPosition, size: fontSize, font: thaiFont });
      //   page.drawText(`${data.bank_account_number}`, { x: 90, y: yPosition, size: fontSize, font: thaiFont });
      //   // const yNameStart = yStart + 20;
      //   page.drawText(`${data.emp_code}`, { x: 220, y: yPosition, size: fontSize, font: thaiFont });
      //   // const yPriceStart = yNameStart + 20;
      //   page.drawText(`${data.name}`, { x: 300, y: yPosition, size: fontSize, font: thaiFont });
      //   page.drawText(`${formatter.format(data.total_allowance)}`, { x: 500, y: yPosition, size: fontSize, font: thaiFont });
      //   yPosition -= descriptionHeight; // Adjust x-position for the next entry
      //   count++
      //   if (count > datas.length - 1) {
      //     console.log('CountSumBefore', this.sumValue)
      //     // if (count === datas.length) {
      //     //   // console.log('CountSum', this.sumValue)
      //     //   this.sumValue = await datas.reduce(function (_this, val) {
      //     //     return _this + parseFloat(val.total_allowance)
      //     //   }, 0);
      //     //   // console.log('CountSum', this.sumValue)
      //     //   // console.log('CountSum', await datas.reduce((acc, obj) => acc += parseInt(obj.total_allowance), 0))
      //     // }
      //     console.log('countPDF ', count);
      //     page.drawText(`__________________________________________________________________________________`, { x: 10, y: yPosition + 10, size: 20, font: thaiFont });
      //     page.drawText(`รวม ${formatter.format(sumValue)}`, { x: 470, y: yPosition - 20, size: 20, font: thaiFont });
      //   }
      // }

      // Save the PDF to a file or display it in a new tab
      const pdfBytes = await pdfDoc.save();
      const blob = new Blob([pdfBytes], { type: 'application/pdf' });
      const url = URL.createObjectURL(blob);
      window.open(url, '_blank');
    },
  }
}
</script>