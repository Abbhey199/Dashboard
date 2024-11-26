};
const response = await fetch(
"https://a.khalti.com/api/v2/epayment/initiate/",
{
method: "POST",
headers: {
Authorization: `Key ${process.env.NEXT_PUBLIC_KHALTI_SECRET_KEY}`,
"Content-Type": "application/json",
},
body: JSON.stringify(khaltiConfig),
}
);
if (!response.ok) {
const errorData = await response.json();
console.error("Khalti API Error:", errorData);
throw new Error(
`Khalti payment initiation failed: ${JSON.stringify(errorData)}`
);
}
const khaltiResponse = await response.json();
console.log("Khalti payment initiated:", khaltiResponse);
return NextResponse.json({
khaltiPaymentUrl: khaltiResponse.payment_url,
});
}
default:
console.error("Invalid payment method:", method);
return NextResponse.json(
{ error: "Invalid payment method" },
{ status: 400 }
);
}
} catch (err) {
console.error("Payment API Error:", err);
return NextResponse.json(
{
error: "Error creating payment session",
details: err instanceof Error ? err.message : "Unknown error",
},
{ status: 500 }
);
}
}# Dashboard