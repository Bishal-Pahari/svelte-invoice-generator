<script>
  import jsPDF from 'jspdf';
  import html2canvas from 'html2canvas';

  let invoiceContent;

  const generatePDF = async () => {
    if (invoiceContent) {
      const canvas = await html2canvas(invoiceContent);
      const imgData = canvas.toDataURL('image/png');

      const doc = new jsPDF({
        orientation: 'portrait',
        unit: 'mm',
        format: [210, 297],
      });

      doc.addImage(imgData, 'PNG', 10, 10, 190, 277);
      doc.save('invoice.pdf');
    } else {
      console.error('Invoice content not available');
    }
  };

  // Sample data
  let invoice = {
    company: {
      name: 'Hamro Patro Remit',
      address: 'Sifal Road, Sifal, KTM',
      email: 'support@hamropatro.com',
      phone: '(111) 987-6543',
    },
    invoiceDetails: {
      number: 'INV-67890',
      date: '2024-09-06',
    },
    customer: {
      name: 'Bishal Pahari',
      address: 'Elden Street 123, Metropolis, CA',
      email: 'bpahari@google.com',
      phone: '(111) 987-6543',
    },
    items: [
      {
        description: 'Software License',
        quantity: 1,
        unitPrice: 1200.0,
        total: 1200.0,
      },
      {
        description: 'Support Package',
        quantity: 3,
        unitPrice: 150.0,
        total: 450.0,
      },
    ],
    subtotal: 1650.0,
    tax: 165.0,
    total: 1815.0,
  };
</script>

<button
  on:click={generatePDF}
  class="block mx-auto my-5 py-2 px-4 text-lg bg-blue-500 text-white rounded shadow hover:bg-blue-700"
>
  Generate PDF
</button>

<div
  bind:this={invoiceContent}
  class=" mx-auto w-[655px] h-[902px] p-5 bg-white border border-gray-300 text-gray-800"
>
  <header class="flex justify-between border-b-2 border-gray-300 pb-5 mb-5">
    <div class="space-y-2">
      <h1 class="text-2xl font-bold">{invoice.company.name}</h1>
      <p>{invoice.company.address}</p>
      <p>Email: {invoice.company.email}</p>
      <p>Phone: {invoice.company.phone}</p>
    </div>
    <div class="text-right">
      <h2 class="text-xl font-bold">Invoice</h2>
      <p><strong>Invoice Number:</strong> {invoice.invoiceDetails.number}</p>
      <p><strong>Date:</strong> {invoice.invoiceDetails.date}</p>
    </div>
  </header>

  <section class="border-b-2 border-gray-300 pb-5 mb-5">
    <h3 class="text-lg font-semibold mb-2">Customer Details</h3>
    <p><strong>Name:</strong> {invoice.customer.name}</p>
    <p><strong>Address:</strong> {invoice.customer.address}</p>
    <p><strong>Email:</strong> {invoice.customer.email}</p>
    <p><strong>Phone:</strong> {invoice.customer.phone}</p>
  </section>

  <section>
    <h3 class="text-lg font-semibold mb-2">Invoice Items</h3>
    <table class="w-full border-collapse mb-5">
      <thead>
        <tr>
          <th class="p-2 border border-gray-300 bg-gray-200 text-left"
            >Description</th
          >
          <th class="p-2 border border-gray-300 bg-gray-200 text-right"
            >Quantity</th
          >
          <th class="p-2 border border-gray-300 bg-gray-200 text-right"
            >Unit Price</th
          >
          <th class="p-2 border border-gray-300 bg-gray-200 text-right"
            >Total</th
          >
        </tr>
      </thead>
      <tbody>
        {#each invoice.items as item}
          <tr>
            <td class="p-2 border border-gray-300">{item.description}</td>
            <td class="p-2 border border-gray-300 text-right"
              >{item.quantity}</td
            >
            <td class="p-2 border border-gray-300 text-right"
              >${item.unitPrice.toFixed(2)}</td
            >
            <td class="p-2 border border-gray-300 text-right"
              >${item.total.toFixed(2)}</td
            >
          </tr>
        {/each}
      </tbody>
      <tfoot>
        <tr>
          <td
            colspan="3"
            class="p-2 border border-gray-300 text-right font-bold"
            >Subtotal:</td
          >
          <td class="p-2 border border-gray-300 text-right"
            >${invoice.subtotal.toFixed(2)}</td
          >
        </tr>
        <tr>
          <td
            colspan="3"
            class="p-2 border border-gray-300 text-right font-bold"
            >Tax (10%):</td
          >
          <td class="p-2 border border-gray-300 text-right"
            >${invoice.tax.toFixed(2)}</td
          >
        </tr>
        <tr>
          <td
            colspan="3"
            class="p-2 border border-gray-300 text-right font-bold">Total:</td
          >
          <td class="p-2 border border-gray-300 text-right"
            >${invoice.total.toFixed(2)}</td
          >
        </tr>
      </tfoot>
    </table>
  </section>

  <footer class="text-center text-sm text-gray-600">
    <p>Thank you for your business!</p>
    <p>Payment terms: Due within 30 days</p>
  </footer>
</div>
