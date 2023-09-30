CREATE TABLE CASE_RECORD (
case_id VARCHAR(25),
req_adj_amount DOUBLE NOT NULL,
complaint_date DATETIME(0) NOT NULL,
req_adj_code VARCHAR(5) NOT NULL,
complaint_type VARCHAR(15) NOT NULL,
deadline DATETIME(0) NOT NULL,
org_txn_id VARCHAR(35) NOT NULL,
created_date DATETIME(0) NOT NULL,
case_number VARCHAR(50) NOT NULL,
duplicate_check BIT(1) DEFAULT 0,
resolved_date DATETIME(0),
reason_code_desc VARCHAR(50),
reason_code_desc_id VARCHAR(4),
initiation_mode VARCHAR(5),
status_id INT(4),
req_adj_flag VARCHAR(5),
user_id VARCHAR(36),
org_sett_resp_code VARCHAR(5),
current_cycle VARCHAR(3),
req_message_id VARACHAR(36),
res_message_id VARCHAR(36),
note VARCHAR(50),
unique_txn_id VARCHAR(50),
ref_Id VARCHAR(50),
ref_url VARCHAR(50),
cust_ref VARCHAR(15),
purpose VARCHAR(3),
ref_category VARCHAR(3),
adj_code VARCHAR(4),
adj_flag VARCHAR(5),
seq_num SMALLINT(4),
sub_type VARCHAR(15),
PRIMARY KEY (case_id)
FOREIGN KEY (status_id) REFERENCES STATUS_ENTITY(status_id),
FOREIGN KEY (user_id) REFERENCES USER_ENTITY(keycloak_user_id),
);




# Case Fields




# Transaction Fields
CREATE TABLE TRANSACTION_UPI_ENTITY(
    transaction_id varchar(50),
    rrn varchar(36),
    transaction_status varchar(30),
    transaction_amount double,
    transaction_date DATETIME(1),
    transaction_code varchar(50),
    auth_code varchar(10),
    issuer_bin varchar(10),
    acquirer_bin varchar(10),
    upi_transaction_type varchar(36),
    original_transaction_Ref_num varchar(50),
    ifsc varchar(11),
    status_code VARCHAR(20),
    approval_number VARCHAR(6),
    currency VARCHAR(3),
    account_number double,
    address varchar(50),
    code varchar(15),
    reg_name varchar(36),
    PRIMARY KEY (transaction_id)
);



