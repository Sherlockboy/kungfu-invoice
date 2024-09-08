<template>
    <div class="container mx-auto p-10">
        <!-- Form Section -->
        <div class="mb-10">
            <h2 class="text-2xl font-bold mb-4">Квитансия маълумотларини киритинг</h2>
            <form class="grid grid-cols-2 gap-6">
                <div class="col-span-2">
                    <label class="block font-semibold mb-2">КИРИМ ҒАЗНА ОРДЕРИ №:</label>
                    <input v-model="paymentNumber" type="number" class="border border-gray-300 p-2 rounded w-1/2" />
                </div>

                <!-- Payment Amount -->
                <div>
                    <label class="block font-semibold mb-2">Тўлов суммаси (сўм - рақамларда):</label>
                    <input v-model="paymentAmount" type="number" placeholder="120,000"
                        class="border border-gray-300 p-2 rounded w-full" />
                </div>

                <div>
                    <label class="block font-semibold mb-2">Тўлов суммаси (сўм - сўзларда):</label>
                    <input v-model="totalAmount" type="text" placeholder="Бир юз йигирма минг"
                        class="border border-gray-300 p-2 rounded w-full" />
                </div>

                <!-- Payment Date -->
                <div>
                    <label class="block font-semibold mb-2">Тўлов қабул қилинган сана:</label>
                    <input v-model="paymentDate" type="date" class="border border-gray-300 p-2 rounded w-full" />
                </div>

                <!-- Participant Name -->
                <div>
                    <label class="block font-semibold mb-2">Курс қатнашувчисининг ФИО:</label>
                    <input v-model="participantName" type="text" class="border border-gray-300 p-2 rounded w-full" />
                </div>

                <!-- Purpose -->
                <div class="col-span-2">
                    <label class="block font-semibold mb-2">Максади:</label>
                    <div class="flex space-x-4">
                        <input v-model="purposeMonth" type="text" class="border border-gray-300 p-2 rounded w-1/2"
                            placeholder="ойи учун" />
                        <input v-model="purposeCourse" type="text" class="border border-gray-300 p-2 rounded w-1/2"
                            placeholder="курс" />
                    </div>
                </div>

                <!-- Instructor -->
                <div>
                    <label class="block font-semibold mb-2">Мураббий:</label>
                    <input v-model="instructor" type="text" class="border border-gray-300 p-2 rounded w-full" />
                </div>

                <!-- Cashier -->
                <div>
                    <label class="block font-semibold mb-2">Пулни қабул қилган шахс:</label>
                    <input v-model="cashier" type="text" class="border border-gray-300 p-2 rounded w-full" />
                </div>
            </form>
        </div>

        <!-- Invoice Layout Section (preview) -->
        <div ref="invoiceSection" class="p-10 bg-white border border-gray-300 rounded-lg max-w-3xl mx-auto">
            <div class="grid grid-cols-3 gap-4 items-center justify-start mt-4 mb-6">
                <div>
                    <img src="../assets/kungfu-logo.png" alt="Logo" class="h-24 mr-auto" />
                </div>
                <div class="col-span-2">
                    <h2 class="text-xl font-bold">КИРИМ ҒАЗНА ОРДЕРИ № <span class="underline">{{ paymentNumber
                            }}</span></h2>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-6">
                <div>
                    <label class="font-bold">Тўлов суммаси:</label>
                    <div class="font-semibold underline p-2 rounded">{{ paymentAmount.toLocaleString('en-US') }} сўм ({{
                        totalAmount
                        }})</div>
                </div>
                <div>
                    <label class="font-bold">Тўлов қабул қилинган сана:</label>
                    <div class="font-semibold underline p-2 rounded">{{ paymentDate }}</div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-4">
                <div>
                    <label class="font-bold">Курс қатнашувчисининг ФИО:</label>
                    <div class="font-semibold underline p-2 rounded">{{ participantName }}</div>
                </div>
                <div>
                    <label class="font-bold">Мураббий:</label>
                    <div class="font-semibold underline p-2 rounded">{{ instructor }}</div>
                </div>
            </div>

            <div class="mb-4">
                <label class="font-bold">Максади:</label>
                <div class="p-2 rounded">
                    <span class="font-semibold underline">{{ purposeMonth }}</span> ойи учун <span
                        class="font-semibold underline">{{ purposeCourse
                        }}</span> ўқув курсига тўлов қилди
                </div>
            </div>

            <div class="grid grid-cols-2 items-center justify-start gap-4 mb-4">
                <div>
                    <label class="font-bold">Пулни қабул қилган шахс:</label>
                    <div class="font-semibold underline p-2 rounded">{{ cashier }}</div>
                </div>
                <div>
                    <img src="../assets/stamp.png" alt="stamp" class="h-36 mr-auto" />
                </div>
            </div>
        </div>

        <!-- Button to generate PDF -->
        <div class="text-center mt-10">
            <button @click="generatePDF" class="bg-green-500 text-white px-6 py-2 rounded-lg hover:bg-green-600">
                Квитансияни юклаш
            </button>
        </div>
    </div>
</template>

<script>
import jsPDF from 'jspdf';
import html2canvas from 'html2canvas';

export default {
    data() {
        return {
            paymentNumber: 0,
            paymentAmount: '',
            paymentDate: '',
            participantName: '',
            purposeMonth: '',
            purposeCourse: '',
            instructor: '',
            cashier: '',
            totalAmount: '',
        };
    },
    methods: {
        async generatePDF() {
            const invoiceElement = this.$refs.invoiceSection;

            // Use html2canvas to capture the section as an image
            const canvas = await html2canvas(invoiceElement, { scale: 2 });
            const imgData = canvas.toDataURL('image/png');

            // Create a PDF using jsPDF
            const pdf = new jsPDF('p', 'mm', 'a4');
            const imgWidth = 210; // A4 width in mm
            const pageHeight = 295; // A4 height in mm

            // Define margins (in mm)
            const margin = 10; // Example margin size

            // Calculate the available width and height for the image
            const availableWidth = imgWidth - 2 * margin;
            const availableHeight = pageHeight - 2 * margin;

            // Calculate the aspect ratio and scale the image
            const canvasWidth = canvas.width;
            const canvasHeight = canvas.height;
            const imgAspectRatio = canvasWidth / canvasHeight;
            const pageAspectRatio = availableWidth / availableHeight;

            let imgScaledWidth, imgScaledHeight;

            if (imgAspectRatio > pageAspectRatio) {
                // Image is wider relative to its height, so fit to width
                imgScaledWidth = availableWidth;
                imgScaledHeight = (availableWidth / imgAspectRatio);
            } else {
                // Image is taller relative to its width, so fit to height
                imgScaledHeight = availableHeight;
                imgScaledWidth = (availableHeight * imgAspectRatio);
            }

            // Calculate position to center image with margin
            const xOffset = margin + (availableWidth - imgScaledWidth) / 2;
            const yOffset = margin + (availableHeight - imgScaledHeight) / 2;

            // Add the captured image to the PDF with margin
            pdf.addImage(imgData, 'PNG', xOffset, yOffset, imgScaledWidth, imgScaledHeight);

            // Save the PDF
            pdf.save(this.generateInvoiceFileName(this.participantName, this.paymentDate));
        },
        generateInvoiceFileName(studentFullName, invoiceDate) {
            // Function to sanitize the student's name by replacing non-alphanumeric characters with underscores
            const sanitizeName = (name) => {
                return name.replace(/[^a-zA-Z0-9]/g, '_');
            };

            // Validate and format the date
            const formatDate = (date) => {
                // Check if date format is YYYY-mm-dd
                const datePattern = /^\d{4}-\d{2}-\d{2}$/;
                return datePattern.test(date) ? date : 'unknown_date';
            };

            // Sanitize the student full name
            const sanitizedFullName = sanitizeName(studentFullName);

            // Format the invoice date
            const formattedDate = formatDate(invoiceDate);

            // Return the file name in the format "name_date.pdf"
            return `${sanitizedFullName}_${formattedDate}.pdf`;
        }
    },
};
</script>

<style scoped>
body {
    font-family: 'Arial', sans-serif;
}
</style>