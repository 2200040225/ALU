# ALU

Design code:

module ALU (
    input [3:0] A,        
    input [3:0] B,        
    input [2:0] ALU_Sel,  
    output reg [3:0] ALU_Out, 
    output reg Carry_Out      
);

always @(*) begin
    Carry_Out = 0;
    case (ALU_Sel)
        3'b000: {Carry_Out, ALU_Out} = A + B; // Addition
        3'b001: {Carry_Out, ALU_Out} = A - B; // Subtraction
        3'b010: ALU_Out = A & B;              // AND
        3'b011: ALU_Out = A | B;              // OR
        3'b100: ALU_Out = A ^ B;              // XOR
        3'b101: ALU_Out = ~A;                 // NOT A
        3'b110: ALU_Out = B << 1;             // Left Shift B
        3'b111: ALU_Out = B >> 1;             // Right Shift B
        default: ALU_Out = 4'b0000;           // Default case
    endcase
end
endmodule

Test bench:

module ALU_tb;

    reg [3:0] A;          
    reg [3:0] B;          
    reg [2:0] ALU_Sel;     
    wire [3:0] ALU_Out;    
    wire Carry_Out;       
    ALU uut (
        .A(A),
        .B(B),
        .ALU_Sel(ALU_Sel),
        .ALU_Out(ALU_Out),
        .Carry_Out(Carry_Out)
    );

    initial begin
        // Test addition
        A = 4'b0011; B = 4'b0010; ALU_Sel = 3'b000; #10;
        $display("Addition: A = %b, B = %b, ALU_Out = %b, Carry_Out = %b", A, B, ALU_Out, Carry_Out);

        // Test subtraction
        A = 4'b0110; B = 4'b0010; ALU_Sel = 3'b001; #10;
        $display("Subtraction: A = %b, B = %b, ALU_Out = %b, Carry_Out = %b", A, B, ALU_Out, Carry_Out);

        // Test AND
        A = 4'b1100; B = 4'b1010; ALU_Sel = 3'b010; #10;
        $display("AND: A = %b, B = %b, ALU_Out = %b", A, B, ALU_Out);

        // Test OR
        A = 4'b1100; B = 4'b1010; ALU_Sel = 3'b011; #10;
        $display("OR: A = %b, B = %b, ALU_Out = %b", A, B, ALU_Out);

        // Test XOR
        A = 4'b1100; B = 4'b1010; ALU_Sel = 3'b100; #10;
        $display("XOR: A = %b, B = %b, ALU_Out = %b", A, B, ALU_Out);

        // Test NOT A
        A = 4'b1100; ALU_Sel = 3'b101; #10;
        $display("NOT A: A = %b, ALU_Out = %b", A, ALU_Out);

        // Test Left Shift
        B = 4'b0011; ALU_Sel = 3'b110; #10;
        $display("Left Shift B: B = %b, ALU_Out = %b", B, ALU_Out);

        // Test Right Shift
        B = 4'b1010; ALU_Sel = 3'b111; #10;
        $display("Right Shift B: B = %b, ALU_Out = %b", B, ALU_Out);

        $stop;
    end
endmodule

