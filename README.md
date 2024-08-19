// Code your design here
module sync_fifo(
  input [7:0]data_i,
  input wr_en,
  input clk,
  input rst_n,
  input rd_en,
   
  
  output reg [7:0]data_o,
  output full,
  output empty
);
  parameter DEPTH=8;
  
  reg [7:0]mem [0:DEPTH-1];
  
  reg [2:0]wr_ptr;
  reg [2:0]rd_ptr;
  reg [3:0]count;
  
  //full and empty conditions
  
  assign full=(count==DEPTH);
  assign empty=(count==0);
  
  
  //handle with write process
  always@(posedge clk or negedge rst_n)
    begin
    if(!rst_n)
      begin
      wr_ptr<=3'b0;
      end
      else
        begin
        if(wr_en && !full)
          begin
            mem[wr_ptr]<=data_i;
            wr_ptr<=wr_ptr+1;
          end
        end
    end
  
  //handle with read process
  always@(posedge clk or negedge rst_n)
    begin
    if(!rst_n)
      begin
      rd_ptr<=3'b0;
      end
      else
        begin
          if(rd_en && !empty)
          begin
            data_o<=mem[rd_ptr];
            rd_ptr<=rd_ptr+1;
          end
        end
    end
  
   //handle with count
  always@(posedge clk or negedge rst_n)
    begin
      if(!rst_n)
        begin
          count<=4'b0;
        end
      else
        begin
          case({wr_en,rd_en})
            2'b10:count<=count+1;
            2'b01:count<=count-1;
            2'b00:count<=count;
            2'b11:count<=count;
            default:count<=count;
          endcase
        end
    end
endmodule
  
  
  
  
  
  
