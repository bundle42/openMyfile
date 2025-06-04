# openMyfile

module fourbit_parallel_adder(a, b, clock, com, data_out, P);

input [3:0] a, b;
input clock;
output [3:0] com;
output [6:0] data_out;
output P;

wire [4:0] f;
wire [3:0] B, A;

// assign f = a + b;
fourbit_adder U0(a[0],a[1],a[2],a[3],b[0],b[1],b[2],b[3],f[4],f[3],f[2],f[1],f[0]);

five_to_2digit U1(f, B, A);

Seg_2digit U2(A, B, clock, com, data_out, P);

endmodule 

--------------------------------

module five_to_2digit(f, B, A);

input [4:0] f;
output [3:0] B, A;

assign B[3] = 0;
assign B[2] = 0;
assign B[1] = (f[4] & f[2]) | (f[4] & f[3]);
assign B[0] = (f[4] & ~f[3] & ~f[2]) | (~f[4] & f[3] & f[2]) | (f[3] & f[2] & f[1]) | (~f[4] & f[3] & f[1]);

assign A[3] = (f[4] & f[3] & f[2] & ~f[1]) | (f[4] & ~f[3] & ~f[2] & f[1]) | (~f[4] & f[3] & ~f[2] & ~f[1]);
assign A[2] = (~f[4] & ~f[3] & f[2]) | (f[4] & f[3] & ~f[2]) | (~f[4] & f[2] & f[1]) | (f[4] & ~f[2] & ~f[1]);
assign A[1] = (~f[4] & ~f[3] & f[1]) | (~f[3] & f[2] & f[1]) | (f[4] & ~f[3] & ~f[2] & ~f[1]) | (~f[4] & f[3] & f[2] & ~f[1]) | (f[4] & f[3] & ~f[2] & f[1]);
assign A[0] = f[0];

endmodule 
