## Sequence of Records and Description

Each NACHA formatted file you originate consists of the following records:

* A File Header Record,
* One or more Company/Batch Header Record(s),
* Entry Detail Record(s),
* Addenda Record(s), if allowed and you choose to include them, or if required One or more Company/Batch Control Record(s) and,
* A File Control Record.

Each file begins with a File Header record. Following the File Header Record may be any number of batches. Each batch is identified by a Batch Header Record and contains one or more Entry Detail Records. At the end of each batch is a Batch Control Record. Each file is ended with a File Control Record.
The diagram on the following page illustrates the Sequence of Records for ACH entries. The sequence of records will always be the same, regardless of SEC code. Out-of- sequence records or lack of a mandatory record will cause all or portions of the file to reject. Padding with “9” records at the end of the file is optional.

## Input File Descriptions

### File Header

The File Header Record designates physical file characteristics and identifies the immediate origin of the entries contained within the file or within the transmitted batched data. In addition, this record includes date, time, and file identification fields that can be used to uniquely identify the file.

### Company/Batch Header Record

The Company/Batch Header Record identifies the Originator and briefly describes the purpose of the entries that are contained within the batch. For example, “GAS BILL” or “REG SALARY” indicates the reason for the transaction originated by the Originator. It also indicates the intended effective entry date of all transactions within the batch. The information contained in the Company/Batch Header Record applies uniformly to all subsequent Entry Detail Records in the batch.
If you wish to vary any of this information, you must create a separate batch. For example, if you are making regular payroll payments and bonus payments then you should create one batch described as “REG SALARY” and another as “BONUS.”

### Entry Detail Record

Entry Detail Records contain information that relate the specific entry to the Receiver, such as the Receiving Depository Financial Institution account and routing transit number and the debit or credit amount.
Prenotifications (prenotes) are special zero-dollar entries used to test the validity of the account number and transit routing number provided by the Receiver. Prenotes are identical to the basic Entry Detail format but contain appropriate Transaction Codes and zeroes in the amount field. Prenotes can be batched with other dollar entries or batched separately.
Zero-dollar entries used in corporate trade payments to deliver remittance information contain appropriate Transaction Codes and zeros in the Amount field but otherwise are formatted the same as other entries. Zero-dollar entries can be batched with other CCD dollar entries or batched separately. One Addenda Record must accompany a CCD zero- dollar entry.

### Addenda Records

For non-IAT entries, Addenda Records are used by the Originator to supply additional information about Entry Detail Records to the Receiver. For many types of entries, such as payroll, addenda records are optional. Addenda Records are usually required for tax payments.

### Company/Batch Control Record

The Company/Batch Control Record contains the counts, hash totals, and total dollar controls for the preceding detail entries within the indicated batch.
All Entry Detail Records are hashed. (The method for calculating hash totals is provided in the Entry Information column in the Record Layouts.) Both Entry Detail Records and Addenda Records are included in the entry/addenda counts; Batch Header and Batch Control Records are not included.

### File Control Record

The File Control Record contains dollar, entry, and hash total accumulations from the Company/Batch Control Records in the file. This record also contains counts of the number of blocks and the number of batches within the file (or batched data transmitted to a single destination).

## NACHA Data Entry Specifications

All alphanumeric and alphabetic fields must be left justified and space filled. All numeric fields must be right justified, unsigned, and zero filled. Characters used in ACH records are restricted to 0-9, A-Z, space, and those special characters which have an EBCDIC value greater than hexadecimal "3F" or an ASCII value greater than hexadecimal "1F.” Occurrences of values EBCDIC "00" - "3F" and ASCII "00" - "1F" are not valid.
Do not use characters that do not meet these requirements.

### Field Inclusion Requirements

The following information defines the requirement for inclusion of certain data fields in ACH entries. These designations are: Mandatory (M), Required (R), and Optional (O).

* **Mandatory.** A “Mandatory” field contains information necessary to ensure the proper routing and/or posting of an ACH entry. The ACH Operator will reject any entry or batch, which does not have appropriate values in a Mandatory field. Bank of America will edit for these same values so that your entries will not reject further down in the processing stream.
* **Required.** The omission of a “Required” field will not cause an entry reject at the ACH Operator, but may cause a reject at the RDFI. For example, if the DFI Account Number field in the Entry Detail Record is omitted, the RDFI may return the entry because it cannot be posted. You should include appropriate values in “Required” fields to avoid processing and control problems at the RDFI.
* **Optional.** The inclusion or omission of an “Optional” data field is at the discretion of the Originator. If you do include optional fields, the RDFI must include them in any return.

## Annotated NACHA Record Formats

### File Header Record - All Formats

The File Header Record designates physical file characteristics. It also identifies the Bank as the immediate destination and your company as the immediate origin of the file.

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '1' | Record Type Code | Code Identifying the File Header Record is '1' | M |
| *2* | 02-03 | 2 | '01' | Priority Code | Currently, only "01" is used | R |
| *3* | 04-13 | 10 | bNNNNNNNNN | Immediate Destination | The Immediate Destination Field identifies the party to which the file is being delivered. Usually the Routing Number of the ACH operator  | M |
| *4* | 14-23 | 10 | NNNNNNNNNN | Immediate Origin | The Immediate Origin Field identifies the sender of the file. | M |
| *5* | 24-29 | 6 | YYMMDD | File Creation Date | The date you create or transmit the input file: <br> “YY” = Last two digits of the Year <br> “MM” = Month in two digits <br> “DD” = Day in two digits | M |
| *6* | 30-33 | 4 | HHMM | File Creation Time | Time of day you create or transmit the input file. This field is used to distinguish among input files if you submit more than one per day: <br>  “HH = Hour based on a 24 hr clock <br >“MM” = Minutes in two digits | O |
| *7* | 34-34 | 1 |  UPPER CASE A-Z (or 0-9) | File ID Modifier | Code to distinguish among multiple input files sent per day. Label the first (or only) file “A” (or “0”) and continue in sequence. | M |
| *8* | 35-37 | 3 | "094" | Record Size | Number of bytes per record-always 94. | M |
| *9* | 38-39 | 2 | "10" | Blocking Factor | Number of records per block | M |
| *10* | 40-40 | 1 | "1" | Format Code | Currently only “1” is used | M |
| *11* | 41-63 | 23 | Alphanumeric | Immediate Destination Name | This field contains the name of the ACH Operator or Receiving Point for which that File is destined | M |
| *12* | 64-86 | 23 | Alphanumeric | Immediate Origin or Company Name | This field contains the name of the ACH Operator or Sending Point that is Transmitting the File | M |
| *13* | 87-94 | 8 | Alphanumeric | Reference Code | You may use this field to describe the input file for internal accounting purposes or fill with spaces. | O |

## Company/Batch Header Record Formats

A batch is a collection of like entries within a file. You must use a separate batch if any of the batch-level information, such as effective date or company name or company description changes.

**Batch Header Record - All SECs Except IAT**

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '5' | Record Type Code | Code Identifying the Company /Batch Header Record is '5' | M |
| *2* | 02-03 | 2 | Numeric | Transaction Code | Two-digit code that identifies checking account credits/debits | M |
| *3* | 04-11 | 8 | TTTTAAAA | Receiving DFI Identification | Routing Transit number of the receivers financial institution | M |
| *4* | 12-12 | 1 | Numeric | Check Digit | The ninth character of the RDFI Routing Transit Number. Used to check for transpositions. | M |
| *5* | 13-29 | 17 | Alphameric | DFI Account Number | Receiver's account number at the RDFI, a value found on the MICR line of a check| R |
| *6* | 30-39 | 10 | $$$$$$$$¢¢ | Amount | Entry amount in dollars with two decimal places. | M |
| *7* | 40-54 | 15 | Alphameric | Check Serial Number |The serial number of the check being represented | M |
| *8* | 55-76 | 22 | Alphameric | Individual Name | Receiver's Name | M |
| *9* | 77-78 | 2 | Alphameric | Discretionary Data | The use of this field is defined byu the ODFI | O |
| *10* | 79-79 | 1 | Numeric | Addenda Record Indicator | "0" = no addenda <br>"1" = one addenda included | M |
| *11* | 80-94 | 15 | Numeric | Trace Number | Standard Entry Detail Trace Number | M |


### RCK Represented Check Entries

RCK Entry Detail Record is a physical check that was presented but returned because of insufficient funds may be represented as an ACH entry.

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '6' | Record Type Code | Code Identifying the Entry Detail Record is '6' | M |
| *2* | 02-03 | 2 | Numeric | Transaction Code | Two-digit code that identifies checking account credits/debits | M |
| *3* | 04-11 | 8 | TTTTAAAA | Receiving DFI Identification | Routing Transit number of the receivers financial institution | M |
| *4* | 12-12 | 1 | Numeric | Check Digit | The ninth character of the RDFI Routing Transit Number. Used to check for transpositions. | M |
| *5* | 13-29 | 17 | Alphameric | DFI Account Number | Receiver's account number at the RDFI, a value found on the MICR line of a check| R |
| *6* | 30-39 | 10 | $$$$$$$$¢¢ | Amount | Entry amount in dollars with two decimal places. | M |
| *7* | 40-54 | 15 | Alphameric | Check Serial Number |The serial number of the check being represented | M |
| *8* | 55-76 | 22 | Alphameric | Individual Name | Receiver's Name | M |
| *9* | 77-78 | 2 | Alphameric | Discretionary Data | The use of this field is defined byu the ODFI | O |
| *10* | 79-79 | 1 | Numeric | Addenda Record Indicator | "0" = no addenda <br>"1" = one addenda included | M |
| *11* | 80-94 | 15 | Numeric | Trace Number | Standard Entry Detail Trace Number | M |

## POP Point-of-Purchase

**Point-of-Purchase Entry.** A check presented in-person to a merchant for purchase is presented as an ACH entry instead of a physical check.

This ACH debit application is used by originators as a method of payment for the in-person purchase of goods or services by consumers. These Single Entry debit entries are initiated by the originator based on a written authorization and account information drawn from the source document (a check) obtained from the consumer at the point-of-purchase. The source document, which is voided by the merchant and returned to the consumer at the point-of-purchase, is used to collect the consumer’s routing number, account number and check serial number that will be used to generate the debit entry to the consumer’s account.

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '6' | Record Type Code | Code Identifying the Entry Detail Record is '6' | M |
| *2* | 02-04 | 3 | '200'<br>'220'<br>'225' | Service Class Code | Identifies the type of entries in the batch. Code “200” indicates a mixed batch, i.e., one containing debit and/or credit entries; “220” is for credits only; “225” is for debits only. | M |
| *3* | 05-20 | 16 | Alphanumeric | Company Name | Your company name. NACHA rules require the RDFI to print this value on the receiver‟s statement so you will want to make this value as clear as possible. | M |
| *4* | 21-40 | 20 | Alphanumeric | Company Discretionary Data | For your company's internal use. | O |
| *5* | 41-50 | 10 | NNNNNNNNN | Company Identification | The Company Identification field carries the Originator’s unique identification number. | M |
| *6* | 51-53 | 3 | 'WEB', 'PPD', etc | Standard Entry Class Code | A mnemonic, designated by NACHA, which permits entries to be distinguished. Identifies the specific record format used to carry payment and payment-related information. | M |
| *7* | 54-63 | 10 | Alphameric | Company Entry Description | You establish the value of this field to provide a description to be displayed to the Receiver. Description should describe the purpose of the entries, such as “PAYROLL” or “ECHECKPAY” for consumer entries or “TRADE PAY” for corporate receivers. NACHA Rules require that RDFIs print this value on the Receiver's account statement. | M |
| *8* | 64-69 | 6 | Alphameric | Company Descriptive Date | Description you choose to identify the date. NACHA recommends, but does not require, that RDFIs print this value on the receiver's statement. | O |
| *9* | 70-75 | 6 | YYMMDD | Effective Entry Date | Date you desire funds to post to receiver's account in YYMMDD format. | R |
| *10* | 76-78 | 3 | blanks | Settlement Date (Julian) | The ACH Operator will populate the actual settlement date in this field. | R |
| *11* | 79-79 | 1 | '1' | Originator Status Code | Identifies the Originator as a non-Federal Government entity. | M |
| *12* | 80-87 | 8 | NNNNNNNN | Originating DFI Identification | The Originating institutions Routing Number | M |
| *13* | 88-94 | 7 | Numeric | Batch Number | Assign batch numbers in ascending order within each file. | M |

## BOC Back Office Conversation

**Back Office Conversion Entry.** A single entry debit initiated at the point of purchase or at a manned bill payment location to transfer funds through conversion to an ACH debit entry during back office processing. Unlike ARC entries, BOC conversions require the customer to be present and a notice that checks may be converted to BOC ACH entries be posted.

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '6' | Record Type Code | Code Identifying the Entry Detail Record is '6' | M |
| *2* | 02-03 | 2 | Numeric | Transaction Code | Two-digit code that identifies checking account credits/debits | M |
| *3* | 04-11 | 8 | TTTTAAAA | Receiving DFI Identification | Routing Transit number of the receivers financial institution | M |
| *4* | 12-12 | 1 | Numeric | Check Digit | The ninth character of the RDFI Routing Transit Number. Used to check for transpositions. | M |
| *5* | 13-29 | 17 | Alphameric | DFI Account Number | Receiver's account number at the RDFI, a value found on the MICR line of a check| R |
| *6* | 30-39 | 10 | $$$$$$$$¢¢ | Amount | Entry amount in dollars with two decimal places. | M |
| *7* | 40-54 | 9 | Alphameric | Check Serial Number |The serial number of the check being represented | M |
| *8* | 55-76 | 22 | Alphameric | Individual Name | Receiver's Name | O |
| *9* | 77-78 | 2 | Blank | Discretionary Data | The use of this field is defined byu the ODFI | O |
| *10* | 79-79 | 1 | Numeric | Addenda Record Indicator | "0" = no addenda <br>"1" = one addenda included | O |
| *11* | 80-94 | 15 | Numeric | Trace Number | Standard Entry Detail Trace Number | M |

## ARC Accounts Receivable Entry

**Accounts Receivable Entry.** A consumer check converted to a one-time ACH debit. The difference between ARC and POP is that ARC can result from a check mailed in where as POP is in-person.

The Accounts Receivable (ARC) Entry provides billers the opportunity to initiate single-entry ACH
debits to customer accounts by converting checks at the point of receipt through the U.S. mail, at
a drop box location or in-person for payment of a bill at a manned location. The biller is required
to provide the customer with notice prior to the acceptance of the check that states the receipt of
the customer’s check will be deemed as the authorization for an ARC debit entry to the customer’s
account. The provision of the notice and the receipt of the check together constitute authorization
for the ARC entry. The customer’s check is solely be used as a source document to obtain the routing
number, account number and check serial number. 


| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '6' | Record Type Code | Code Identifying the Entry Detail Record is '6' | M |
| *2* | 02-03 | 2 | Numeric | Transaction Code | Two-digit code that identifies checking account credits/debits | M |
| *3* | 04-11 | 8 | TTTTAAAA | Receiving DFI Identification | Routing Transit number of the receivers financial institution | M |
| *4* | 12-12 | 1 | Numeric | Check Digit | The ninth character of the RDFI Routing Transit Number. Used to check for transpositions. | M |
| *5* | 13-29 | 17 | Alphameric | DFI Account Number | Receiver's account number at the RDFI, a value found on the MICR line of a check| R |
| *6* | 30-39 | 10 | $$$$$$$$¢¢ | Amount | Entry amount in dollars with two decimal places. | M |
| *7* | 40-54 | 9 | Alphameric | Check Serial Number |The serial number of the check being represented | M |
| *8* | 55-76 | 22 | Alphameric | Individual Name | Receiver's Name | O |
| *9* | 77-78 | 2 | Blank | Discretionary Data | The use of this field is defined byu the ODFI | O |
| *10* | 79-79 | 1 | Numeric | Addenda Record Indicator | "0" = no addenda <br>"1" = one addenda included | O |
| *11* | 80-94 | 15 | Numeric | Trace Number | Standard Entry Detail Trace Number | M |

## POS Point-of-Sale

**Point of Sale Entry.** A POS Entry is a debit Entry initiated at an “electronic terminal” to a Consumer Account of the Receiver to pay an obligation incurred in a point- of-sale transaction, or to effect a point-of-sale terminal cash withdrawal.

Point-of-Sale Entries (POS) are ACH debit entries typically initiated by the use of a merchant-issued plastic card to pay an obligation at the point-of-sale. Much like a financial institution issued debit card, the merchant- issued debit card is swiped at the point-of-sale and approved for use; however, the authorization only verifies the card is open, active and within the card’s limits—it does not verify the Receiver’s account balance or debit the account at the time of the purchase. Settlement of the transaction moves from the card network to the ACH Network through the creation of a POS entry by the card issuer to debit the Receiver’s account.

**POS Entry Detail Record**

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '6' | Record Type Code | Code Identifying the Entry Detail Record is '6' | M |
| *2* | 02-03 | 2 | Numeric | Transaction Code | Two-digit code that identifies checking account credits/debits | M |
| *3* | 04-11 | 8 | TTTTAAAA | Receiving DFI Identification | Routing Transit number of the receivers financial institution | M |
| *4* | 12-12 | 1 | Numeric | Check Digit | The ninth character of the RDFI Routing Transit Number. Used to check for transpositions. | M |
| *5* | 13-29 | 17 | Alphameric | DFI Account Number | Receiver's account number at the RDFI, a value found on the MICR line of a check| R |
| *6* | 30-39 | 10 | $$$$$$$$¢¢ | Amount | Entry amount in dollars with two decimal places. | M |
| *7* | 40-54 | 9 | Alphameric | Individual Identification Number |The serial number of the check being represented | O |
| *8* | 55-76 | 22 | Alphameric | Individual Name | Receiver's Name | R |
| *9* | 77-78 | 2 | Alphameric | Card Transaction Type | This code is used by card processors to identify the type of transaction, such as a purchase, cash advance, or reversal. Values for this field are assigned by the major card Organizations. Code Values: <br>01	Purchase of goods or services <br>02	Cash <br>03	Return Reversal <br>11	Purchase Reversal <br>12	Cash Reversal <br>13	Return <br>21	Adjustment <br>99	Miscellaneous Transaction | M |
| *10* | 79-79 | 1 | Numeric | Addenda Record Indicator | "0" = no addenda <br>"1" = one addenda included | M |
| *11* | 80-94 | 15 | Numeric | Trace Number | Standard Entry Detail Trace Number | M |

**POS Addenda Record**

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Entry Detail Record is '7' | M |
| *2* | 02-03 | 2 | '02' | Addenda Type Code | The Addenda Type Code defines the specific interpretation and format for the addenda information contained in the Entry. | M |
| *3* | 04-10 | 7 | Alphameric | Reference Information #1 | This field may be used for additional reference numbers, identification numbers, or codes that the merchant needs to identify the particular transaction or customer. | O |
| *4* | 11-13 | 3 | Alphameric | Reference Information #2 | This field may be used for additional reference numbers, identification numbers, or codes that the merchant needs to identify the particular transaction or customer. | O |
| *5* | 14-19 | 6 | Alphameric | Terminal Identification Code | This field identifies an Electronic terminal with a unique code that allows a terminal owner and/or switching network to identify the terminal at which an Entry originated. | R |
| *6* | 20-25 | 6 | Alphameric | Transaction Serial Number | Entry amount in dollars with two decimal places. | R |
| *7* | 26-29 | 4 | MMDD | Transaction Date | This date, expressed MMDD, identifies the date on which the transaction occurred. | R |
| *8* | 30-35 | 6 | Alphameric | Authorization Code or Card Expiration Date | This field indicates the code that a card authorization center has furnished to the merchant. | O |
| *9* | 36-62 | 27 | Alphameric | Terminal Location | This field identifies the specific location of a terminal (i.e., street names of an intersection, address, etc.) in accordance with the requirements of Regulation E. | R |
| *10* | 63-77 | 15 | Alphameric | Terminal City | Identifies the city in which the electronic terminal is located | R |
| *11* | 78-79 | 2 | Alphameric | Terminal State | Identifies the state in which the electronic terminal is located | R |
| *12* | 80-94 | 15 | Numeric | Trace Number | Standard Entry Detail Trace Number | M |

## SHR Shared Network Entry

**Shared Network Entry.** A SHR entry a debit Entry initiated at an “electronic terminal,” as that term is defined in Regulation E, to a Consumer Account of the Receiver to pay an obligation incurred in a point-of-sale transaction, or to effect a point-of-sale terminal cash withdrawal. Also an adjusting or other credit Entry related to such debit Entry, transfer of funds, or obligation. SHR Entries are initiated in a shared network where the ODFI and RDFI have an agreement in addition to these Rules to process such Entries.

**SHR Entry Detail Record**

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '6' | Record Type Code | Code Identifying the Entry Detail Record is '6' | M |
| *2* | 02-03 | 2 | Numeric | Transaction Code | Two-digit code that identifies checking account credits/debits | M |
| *3* | 04-11 | 8 | TTTTAAAA | Receiving DFI Identification | Routing Transit number of the receivers financial institution | M |
| *4* | 12-12 | 1 | Numeric | Check Digit | The ninth character of the RDFI Routing Transit Number. Used to check for transpositions. | M |
| *5* | 13-29 | 17 | Alphameric | DFI Account Number | Receiver's account number at the RDFI, a value found on the MICR line of a check| R |
| *6* | 30-39 | 10 | $$$$$$$$¢¢ | Amount | Entry amount in dollars with two decimal places. | M |
| *7* | 40-43 | 4 | MMDD | Card Expiration Date | This code is used by cardholder processors and cardholder Financial Institutions to verify that the card remains valid and that certain security procedures required by various card authorization systems have been met. | R |
| *8* | 44-54 | 11 | Numeric | Document Reference Number | This field further defines the transaction in the event of a Receiver’s inquiry. An example is an Electronic sequence number | R |
| *9* | 55-76 | 22 | Numeric | Individual Card Account Number | The Individual Card Account Number is the number assigned by the card issuer and is obtained from the card itself. | R |
| *10* | 77-78 | 2 | Alphameric | Card Transaction Type | This code is used by card processors to identify the type of transaction, such as a purchase, cash advance, or reversal. Values for this field are assigned by the major card Organizations. Code Values: <br>01	Purchase of goods or services <br>02	Cash <br>03	Return Reversal <br>11	Purchase Reversal <br>12	Cash Reversal <br>13	Return <br>21	Adjustment <br>99	Miscellaneous Transaction | M |
| *11* | 79-79 | 1 | Numeric | Addenda Record Indicator | "0" = no addenda <br>"1" = one addenda included | M |
| *12* | 80-94 | 15 | Numeric | Trace Number | Standard Entry Detail Trace Number | M |

**SHR Addenda Record**

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Entry Detail Record is '7' | M |
| *2* | 02-03 | 2 | '02' | Addenda Type Code | The Addenda Type Code defines the specific interpretation and format for the addenda information contained in the Entry. | M |
| *3* | 04-10 | 7 | Alphameric | Reference Information #1 | This field may be used for additional reference numbers, identification numbers, or codes that the merchant needs to identify the particular transaction or customer. | O |
| *4* | 11-13 | 3 | Alphameric | Reference Information #2 | This field may be used for additional reference numbers, identification numbers, or codes that the merchant needs to identify the particular transaction or customer. | O |
| *5* | 14-19 | 6 | Alphameric | Terminal Identification Code | This field identifies an Electronic terminal with a unique code that allows a terminal owner and/or switching network to identify the terminal at which an Entry originated. | R |
| *6* | 20-25 | 6 | Alphameric | Transaction Serial Number | Entry amount in dollars with two decimal places. | R |
| *7* | 26-29 | 4 | MMDD | Transaction Date | This date, expressed MMDD, identifies the date on which the transaction occurred. | R |
| *8* | 30-35 | 6 | MMDD | Authorization Code or Card Expiration Date | This field indicates the code that a card authorization center has furnished to the merchant. | O |
| *9* | 36-62 | 27 | Alphameric | Terminal Location | This field identifies the specific location of a terminal (i.e., street names of an intersection, address, etc.) in accordance with the requirements of Regulation E. | R |
| *10* | 63-77 | 2 | Alphameric | Terminal City | Identifies the city in which the electronic terminal is located | R |
| *11* | 78-79 | 2 | Alphameric | Terminal State | Identifies the state in which the electronic terminal is located | R |
| *12* | 80-94 | 15 | Numeric | Trace Number | Standard Entry Detail Trace Number | M |

## CIE Customer-Initiated Entries 

**Customer-Initiated Entry** (or CIE entry) is a credit entry initiated on behalf of, and upon the instruction of, a consumer to transfer funds to a non-consumer Receiver. CIE entries are usually transmitted to a company for payment of funds that the consumer owes to that company and are initiated by the consumer through some type of online banking product or bill payment service provider. With CIEs, funds owed by the consumer are “pushed” to the biller in the form of an ACH credit, as opposed to the biller’s use of a debit application (e.g., PPD, WEB) to “pull” the funds from a customer’s account.

**CIE Entry Detail Record**

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '6' | Record Type Code | Code Identifying the Entry Detail Record is '6' | M |
| *2* | 02-03 | 2 | Numeric | Transaction Code | Two-digit code that identifies checking account credits/debits | M |
| *3* | 04-11 | 8 | TTTTAAAA | Receiving DFI Identification | Routing Transit number of the receivers financial institution | M |
| *4* | 12-12 | 1 | Numeric | Check Digit | The ninth character of the RDFI Routing Transit Number. Used to check for transpositions. | M |
| *5* | 13-29 | 17 | Alphameric | DFI Account Number | Receiver's account number at the RDFI, a value found on the MICR line of a check| R |
| *6* | 30-39 | 10 | $$$$$$$$¢¢ | Amount | Entry amount in dollars with two decimal places. | M |
| *7* | 40-54 | 15 | Alphameric | Individual Name | This field is entered by the ODFI to provide additional identification for the Receiver and may be helpful in identifying returned Entries. Field 7, rather than Field 8, of the Entry Detail Record is used for the Individual Name | R |
| *8* | 55-76 | 22 | Alphameric | Individual Identification Number | This field contains the accounting number by which the Originator (payor) is known to the Receiver (payee). It is used by the Receiver to update accounts receivable Records. It should be the number shown on an invoice, statement, billhead, notice, or other communication as the reference. Numbers may be policy, customer, invoice, meter, sequence, and/or alphanumeric combinations. Field 8, rather than Field 7, of the Entry Detail Record is used for the Individual Identification Number. | M |
| *9* | 77-78 | 2 | Alphameric | Discretionary Data | This field in the Entry Detail Record allows ODFIs to include codes, of significance to them, to enable specialized handling of the Entry. There is no standardized interpretation for the value of this field. It can either be a single two-character code, or two distinct one-character codes, according to the needs of the ODFI and/or Originator involved. This field must be returned intact for any returned Entry. | O |
| *10* | 79-79 | 1 | Numeric | Addenda Record Indicator | "0" = no addenda <br>"1" = one addenda included | M |
| *11* | 80-94 | 15 | Numeric | Trace Number | Standard Entry Detail Trace Number | M |

**CIE Addenda Record**

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Entry Detail Record is '7' | M |
| *2* | 02-03 | 2 | '05' | Addenda Type Code | The Addenda Type Code defines the specific interpretation and format for the addenda information contained in the Entry. | M |
| *3* | 04-83 | 80 | Alphameric | Payment Related Information | This field contains payment related ANSI ASC X12 data segments to further identify the payment or Transmit additional remittance information. | O |
| *4* | 84-87 | 4 | Numeric | Addenda Sequence Number | This number is consecutively assigned to each Addenda Record following an Entry Detail Record. The first addenda sequence number must always be a “1.” | M |
| *5* | 88-94 | 7 | Numeric | Entry Detail Sequence Number | This field contains the ascending sequence number section of the Entry Detail or Corporate Entry Detail Record’s trace number. This number is the same as the last seven digits of the trace number of the related Entry Detail Record or Corporate Entry Detail Record. | M |

## IAT International ACH Transaction

An IAT entry is a credit or debit ACH entry that is part of a payment transaction involving a financial agency’s office (i.e., depository financial institution or business issuing money orders) that is not located in the territorial jurisdiction of the United States. IAT entries can be made to or from a corporate or consumer account and must be accompanied by seven (7) mandatory addenda records identifying the name and physical address of the Originator, name and physical address of the Receiver, Receiver’s account number, Receiver’s bank identity and reason for the payment.

**IAT Batch Header Record**

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '5' | Record Type Code | The code identifying the Company/Batch Header Record | M |
| *2* | 02-04 | 3 | '200'<br>'220'<br>'225' | Service Class Code | Identifies the type of entries in the batch. <br>“200” = mixed debits and credits <br>“220” = credits only<br>“225” = debits only | M |
| *3* | 05-20 | 16 | blank | IAT Indicator | Leave blank | O |
| *4* | 21-22 | 2 | 'FF' | Foreign Exchange Indicator | Fixed-to-Fixed - No currency conversion. Entry is originated in a fixed-value amount and is to be received in the same fixed-value amount in the same currency. Fixed-value entries will have spaces in the Foreign Exchange Reference field. | M |
| *5* | 23-23 | 1 | '3' | Foreign Exchange Reference Indicator | Code used to indicate the content of the Foreign Exchange Reference Field.<br>“3” = Space filled | R |
| *6* | 24-38 | 15 | blank | Foreign Exchange Reference | Space filled | R |
| *7* | 39-40 | 2 | Alphanumeric | ISO Destination Country Code | This field contains the two-character code, as approved by the International Organization for Standardization (ISO), to identify the country in which the entry is to be received. Values can be found on the International Organization for Standardization website: www.iso.org | M |
| *8* | 41-50 | 10 | Alphanumeric | Originator Identification | For U.S. entities: the number assigned will be your tax ID<br>For non-U.S. entities: the number assigned will be your DDA number, or the last 9 characters of your account number if it exceeds 9 characters | M |
| *9* | 51-53 | 3 | 'IAT' | Standard Entry Class Code | A mnemonic, designated by NACHA, for International ACH Transactions | M |
| *10* | 54-63 | 10 | Alphanumeric | Company Entry Description | You establish the value of this field to provide a description to be displayed to the Receiver. Should describe the purpose of the entry, such as “PAYROLL” or “TRADE PAY | M |
| *11* | 64-66 | 3 | Alphanumeric | ISO Originating Currency Code (Account Currency) | This field contains the three-character code, as approved by the International Organization for Standardization (ISO), to identify the currency denomination in which the entry was first originated. If the source of funds is within the territorial jurisdiction of the U.S., enter 'USD', otherwise refer to International Organization for Standardization website for value: www.iso.org | M |
| *12* | 67-69 | 3 | Alphanumeric | ISO Destination Currency Code (Payment Currency) | This field contains the three-character code, as approved by the International Organization for Standardization (ISO), to identify the currency denomination in which the entry will ultimately be settled. If the final destination of funds is within the territorial jurisdiction of the U.S., enter “USD”, otherwise refer to International Organization for Standardization website for value: www.iso.org | M |
| *13* | 70-75 | 6 | YYMMDD | Effective Entry Date | Date you desire funds to post to receiver‟s account | M |
| *14* | 76-78 | 3 | blanks | Settlement Date (Julian) | The ACH Operator will populate the actual settlement date in this field. | M |
| *15* | 79-79 | 1 | '1' | Originator Status Code | Identifies the Originator as a non-Federal Government entity. | M |
| *16* | 80-87 | 8 | TTTTAAAA | Gateway Operator Identification/ ODFI Identification | For Inbound IAT Entries, this field contains the routing number of the U.S. Gateway Operator. For Outbound IAT Entries, this field contains the standard routing number, as assigned by Accuity, that identifies the U.S. ODFI initiating the Entry. | M |
| *17* | 88-94 | 7 | Numeric | Batch Number | Assign batch number in ascending order in each batch | M |

**NOTE**: For IAT Return Entries, each field of the Company Batch Header Record remains unchanged from the original record, except:

1) Foreign Exchange Reference - For the return of an outbound International ACH transaction originated by a U.S. ODFI, this field will contain the foreign exchange rate that is applicable at the time of the return entry if a foreign exchange rate is provided within this field on the forward entry.

2) Originator Status Code - Changed to reflect the Originator Status Code of the institution initiating the Return Entry (i.e., the RDFI of the original entry).

3) Gateway Operator Identification/ ODFI Identification - Changed to reflect the Routing number of the institution initiating the Return Entry (i.e., the RDFI of the original entry).

4) Batch Number - Changed to the batch number assigned by the institution preparing the Automated Return Entry.

**IAT Entry Detail**

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '6' | Record Type Code | Code Identifying the Entry Detail Record is '6' | M |
| *2* | 02-03 | 2 | Numeric | Transaction Code | Two-digit code that identifies checking account credits/debits | M |
| *3* | 04-11 | 8 | TTTTAAAA | Receiving DFI Identification | Routing Transit number of the receivers financial institution | M |
| *4* | 12-12 | 1 | Numeric | Check Digit | The ninth character of the RDFI Routing Transit Number. Used to check for transpositions. | M |
| *5* | 13-16 | 4 | Numeric | Number of Addenda Records | Number of Addenda Records | M |
| *6* | 17-29 | 13 | blank | Reserved | Leave blank | M |
| *7* | 30-39 | 10 | $$$$$$$$¢¢ | Amount | Entry amount in dollars with two decimal places. | M |
| *8* | 40-74 | 35 | Alphanumeric | Foreign Receiver's Account Number/DFI Account Number | Receiver‟s account number at the RDFI domiciled in the territorial jurisdiction of the U.S. A value generally found on the MICR line of a check. Enter the MICR Dash Cue Symbol as a hyphen (“-”). Account numbers vary in format. If the account number has less than 35 characters, left-justify, blank-fill. Ignore any blank spaces within the account number. | M |
| *9* | 75-76 | 2 | blank | Reserved | Leave Blank | n/a |
| *10* | 77-77 | 1 | blank | Gateway Operator OFAC screening indicator | Assigned by the ACH Gateway operator | O |
| *11* | 78-78 | 1 | blank | Secondary OFAC screening indicator | Assigned by the ACH Gateway operator | O |
| *12* | 79-79 | 1 | "1" | Addenda Record indicator | "1" = Addenda records follow this entry | M |
| *13* | 80-94 | 15 | Numeric | Trace Number | The field is constructed as follows: <br> Positions 80-87 should be the same as Field 16 of the IAT Company/Batch Header. Positions 88- 94 are filled with the Entry Detail Sequence Number. This number must be assigned in ascending order to entries within each batch, although the numbers need not be continuous. | M |

**NOTE**: For IAT Return Entries, each field of the Entry Detail Record remains unchanged from the original entry, except: 
1) Transaction Code -Changed to the appropriate Return Entry Transaction Code.
2) Receiving DFI Identification - Changed to the routing number of the institution receiving the return entry (i.e., OFI of original entry).
3) Check Digit - Changed to the check digit according to NACH standards and based on the Routing Number contained in position 04-11.
4) Amount - For the return of an outbound International ACH Transaction originated by a U.S ODFI, this amount will be different from the amount reflected in the original forward entry if the exchange rate is different at the time of the return.
5) Trace Number - Changed to the trace number assigned by the institution preparing the Automated Return Entry.

**IAT Addenda Records**

For IAT entries, the first seven Addenda Records are mandatory and contain additional information about the receiver, the ODFI and the RDFI as required by OFAC. The type '717' remittance addenda is optional and is used by the Originator to supply additional information about Entry Detail Record to the Receiver; a maximum of two type '717' addenda are allowed. The type '718' Foreign Correspondent Bank addenda must be included if a Foreign Correspondent Bank is involved in the processing of the IAT entry; a maximum of five type '718' addenda are allowed.
Each Addenda Record must be included in the item count that is entered into the Company/Batch Control Record.

**IAT First Addenda Record (710)**

The First Addenda Record identifies the Receiver of the transaction and the dollar amount of the payment.

 Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Addenda Record | M |
| *2* | 02-03 | 2 | '10' | Addenda Type Code | First Addenda REcord for IAT | M | 
| *3* | 04-06 | 3 | Alphanumeric | Transaction Type Code | Describes the type of payment:<br> ANN = Annuity <br>BUS = Business/Commercial <br>DEP = Deposit<br>LOA = Loan<br>MIS = Miscellaneous<br>MOR = Mortgage<br>PEN = Pension<br>RLS = Rent/Lease<br>REM = Remittance2<br>SAL = Salary/Payroll<br>TAX = Tax<br>TEL = Telephone-Initiated Transaction <br>WEB = Internet-Initiated Transaction <br>ARC = Accounts Receivable Entry <br>BOC = Back Office Conversion Entry <br>POP = Point of Purchase Entry<br>RCK = Re-presented Check Entry | R |
| *4* | 07-24 | 18 | $$$$$$$$$$$$$$$$¢¢ | Foreign Payment Amount | For inbound IAT payments this field should contain the USD amount or may be blank. | R |
| *5* | 25-46 | 22 | Alphanumeric | Foreign Trace Number | Insert blanks or zeros| O |
| *6* | 47-81 | 35 | Alphanumeric | Receiving Company Name/Individual Name | Insert receiver's name | M |
| *7* | 82-87 | 6 | blank | Reserved | Leave blank | n/a |
| *8* | 88-94 | 7 | Alphanumeric | Entry Detail Sequence Number | This field contains the ascending sequence number section of the Entry Detail Record‟s trace number. This number is the same as the last seven digits of the trace number (Field 13) of the related Entry Detail Record. | M |

**NOTE**: For IAT Return Entries, each field of the 1st Addenda Record remains unchanged from the original 1st Addenda Record, except Entry Detail Sequence Number which reflects the Entry Detail Sequence Number assocoiated with the trace number assigned by the institituion preparing the Automated Return Entry.

**IAT Second Addenda Record (711)**

The Second and Third Addenda Records identify key information related to the Originator of the entry.

 Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Addenda Record | M |
| *2* | 02-03 | 2 | '11' | Addenda Type Code | Second Addenda Record for IAT | M | 
| *3* | 04-38 | 35 | Alphanumeric | Originator Name | Contains the originators name (your company name) | M |
| *4* | 39-73 | 35 | Alphanumeric | Originator Street Address | Contains the originators street address (your company's address) | m |
| *5* | 74-87 | 14 | blank | Reserved | Leave blank | n/a |
| *6* | 88-94 | 7 | Alphanumeric | Entry Detail Sequence Number | This field contains the ascending sequence number section of the Entry Detail Record‟s trace number. This number is the same as the last seven digits of the trace number (Field 13) of the related Entry Detail Record. | M |

**NOTE**: For IAT Return Entries, each field of the 2nd Addenda Record remains unchanged from the original 2nd Addenda Record, except Entry Detail Sequence Number whihch reflects the Entry Detail Sequence Number assocoiated with the trace number assigned by the institituion preparing the Automated Return Entry.

**IAT Third Addenda Record (712)**

 Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Addenda Record | M |
| *2* | 02-03 | 2 | '12' | Addenda Type Code | Third Addenda Record for IAT | M | 
| *3* | 04-38 | 35 | Alphanumeric | Originator City & State / Province | City and State should be separated with an asterisk (*) as a delimiter and the field should end with a backslash (\).<br>For example: San Francisco*CA\. | M |
| *4* | 39-73 | 35 | Alphanumeric | Originator Country & Postal Code | Data elements must be separated by an asterisk (*) and must end with a backslash (\) <br>For example: US*10036\ | M |
| *5* | 74-87 | 14 | blank | Reserved | Leave blank | n/a |
| *6* | 88-94 | 7 | Alphanumeric | Entry Detail Sequence Number | This field contains the ascending sequence number section of the Entry Detail Record‟s trace number. This number is the same as the last seven digits of the trace number (Field 13) of the related Entry Detail Record. | M |

**NOTE**: For IAT Return Entries, each field of the 3rd Addenda Record remains unchanged from the original 3rd Addenda Record, except Entry Detail Sequence Number whihch reflects the Entry Detail Sequence Number assocoiated with the trace number assigned by the institituion preparing the Automated Return Entry.

**IAT Fourth Addenda Record (713)**

The Fourth Addenda Record contains information related to the financial institution originating the entry. For inbound IAT entries, the Fourth Addenda Record must contain information to identify the foreign financial institution that is providing the funding and payment instruction for the IAT entry.

 Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Addenda Record | M |
| *2* | 02-03 | 2 | '13' | Addenda Type Code | Fourth Addenda Record for IAT | M | 
| *3* | 04-38 | 35 | Alphanumeric | Originating DFI Name | For Outbound IAT Entries, this field must contain the name of the U.S. ODFI.<br> For Inbound IATs: Name of the foreign bank providing funding for the payment transaction | M |
| *4* | 39-40 | 2 | "01" | Originating DFI Identification Number Qualifier | “01” = National Clearing System <br>*For Inbound IATs:* The 2-digit code that identifies the numbering scheme used in the Foreign DFI Identification Number field: <br>01 = National Clearing System <br>02 = BIC Code <br>03 = IBAN Code | M |
| *5* | 41-74 | 34 | Alphanumeric | Originating DFI Identification | This field contains the routing number that identifies the U.S. ODFI initiating the entry.<br>*For Inbound IATs:* This field contains the bank ID number of the Foreign Bank providing funding for the payment transaction. | M |
| *6* | 75-77 | 3 | Alphanumeric | Originating DFI Branch Country Code | USb” = United States<br>(“b” indicates a blank space)<br>*For Inbound IATs:* This 3 position field contains a 2-character code as approved by the International Organization for Standardization (ISO) used to identify the country in which the branch of the bank that originated the entry is located. Values for other countries can be found on the International Organization for Standardization website: www.iso.org. | M |
| *7* | 78-87 | 10 | blank | Reserved | Leave blank | n/a |
| *8* | 88-94 | 7 | Numeric | Entry Detail Sequence Number | This field contains the ascending sequence number section of the Entry Detail Record‟s trace number. This number is the same as the last seven digits of the trace number (Field 13) of the related Entry Detail Record. | M |

**NOTE**: For IAT Return Entries, each field of the 4th Addenda Record remains unchanged from the original 4th Addenda Record, except Entry Detail Sequence Number whihch reflects the Entry Detail Sequence Number assocoiated with the trace number assigned by the institituion preparing the Automated Return Entry.

**IAT Fifth Addenda Record (714)**

The Fifth Addenda Record identifies the Receiving financial institution holding the Receiver's account.

 Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Addenda Record | M |
| *2* | 02-03 | 2 | '14' | Addenda Type Code | Fifth Addenda Record for IAT | M | 
| *3* | 04-38 | 35 | Alphanumeric | Receiving DFI Name | Name of the Receiver's bank | M |
| *4* | 39-40 | 2 | Numeric | Receiving DFI Identification Number Qualifier | The 2-digit code that identifies the numbering scheme used in the Receiving DFI Identification Number field:<br>01 = National Clearing System<br>02 = BIC Code <br>03 = IBAN Code | M |
| *5* | 41-74 | 34 | Alphanumeric | Receiving DFI Identification Number | The bank identification number of the DFI at which the Receiver maintains his account. | M |
| *6* | 75-77 | 3 | Alphanumeric | Receiving DFI Branch Country Code | This 3 position field contains a 2-character code as approved by the International Organization for Standardization (ISO) used to identify the country in which the branch of the bank that receives the entry is located. Values for other countries can be found on the International Organization for Standardization website: www.iso.org<br>“USb” = United States<br>(“b” indicates a blank space) | M |
| *7* | 78-87 | 10 | blank | Reserved | Leave blank | n/a |
| *8* | 88-94 | 7 | Numeric | Entry Detail Sequence Number | This field contains the ascending sequence number section of the Entry Detail Record‟s trace number. This number is the same as the last seven digits of the trace number (Field 13) of the related Entry Detail Record. | M |

**NOTE**: For IAT Return Entries, each field of the 5th Addenda Record remains unchanged from the original 5th Addenda Record, except Entry Detail Sequence Number whihch reflects the Entry Detail Sequence Number assocoiated with the trace number assigned by the institituion preparing the Automated Return Entry.

**IAT Sixth Addenda Record (715)**

The Sixth and Seventh Addenda Records identify information related to the Receiver.

Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Addenda Record | M |
| *2* | 02-03 | 2 | '15' | Addenda Type Code | Sixth Addenda Record for IAT | M | 
| *3* | 04-18 | 15 | Alphanumeric | Receiver Identification Number | This field contains the accounting number by which the Originator is known to the Receiver for descriptive purposes. NACHA Rules recommend but do not require the RDFI to print the contents of this field on the receiver's statement. | O |
| *4* | 19-53 | 35 | Alphanumeric | Receiver Street Address | Receiver‟s physical address | M |
| *5* | 54-87 | 34 | blank | Reserved | Leave blank | n/a |
| *6* | 88-94 | 7 | Numeric | Entry Detail Sequence Number | This field contains the ascending sequence number section of the Entry Detail Record‟s trace number. This number is the same as the last seven digits of the trace number (Field 13) of the related Entry Detail Record. | M |

**NOTE**: For IAT Return Entries, each field of the 6th Addenda Record remains unchanged from the original 6th Addenda Record, except Entry Detail Sequence Number whihch reflects the Entry Detail Sequence Number assocoiated with the trace number assigned by the institituion preparing the Automated Return Entry.

**IAT Seventh Addenda Record (716)**

Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Addenda Record | M |
| *2* | 02-03 | 2 | '16' | Addenda Type Code | Seventh Addenda Record for IAT | M | 
| *3* | 04-38 | 35 | Alphanumeric | Receiver City, State/Province | City and State should be separated with an asterisk (*) as a delimiter and the field should end with a backslash (\).<br>For example: San Francisco*CA\ | M |
| *4* | 39-73 | 35 | Alphanumeric | Receiver Country & Postal Code| Data elements must be separated by an asterisk (*) and must end with a backslash (\) <br>For example: US*10036\ | M |
| *5* | 74-87 | 34 | blank | Reserved | Leave blank | n/a |
| *6* | 88-94 | 7 | Numeric | Entry Detail Sequence Number | This field contains the ascending sequence number section of the Entry Detail Record‟s trace number. This number is the same as the last seven digits of the trace number (Field 13) of the related Entry Detail Record. | M |

**NOTE**: For IAT Return Entries, each field of the 7th Addenda Record remains unchanged from the original 7th Addenda Record, except Entry Detail Sequence Number whihch reflects the Entry Detail Sequence Number assocoiated with the trace number assigned by the institituion preparing the Automated Return Entry.

**IAT Addenda Record for Remittance Information (717) (Optional)**

This is an optional Addenda Record used to provide payment-related data. You may include up to two of these Addenda Records with each IAT entry.

Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Addenda Record | M |
| *2* | 02-03 | 2 | '17' | Addenda Type Code | Seventh Addenda Record for IAT | M | 
| *3* | 04-83 | 80 | Alphanumeric | Payment Related Information | Payment information associated with the preceding Entry Detail Record. Must contain NACHA endorsed ANSI ASC X12 data segments or NACHA endorsed banking conventions. The asterisk (“*”) must be the delimiter between the data elements, and the back slash (“\”) must be the terminator between the data segments. | O |
| *4* | 84-87 | 4 | Numeric | Addenda Sequence Number | Sequence number of each type “17” remittance addenda in ascending order beginning with 0001 | M |
| *5* | 88-94 | 7 | Numeric | Entry Detail Sequence Number | This field contains the ascending sequence number section of the Entry Detail Record‟s trace number. This number is the same as the last seven digits of the trace number (Field 13) of the related Entry Detail Record. | M |

**IAT Addenda Record for Foreign Correspondent Bank Information (718)**

This Addenda Record is used to provide information on each Foreign Correspondent Bank involved in the processing of the IAT entry. If no Foreign Correspondent Bank is involved,the record should not be included.<br>
NOTE: A maximum of five 'Type 18' addenda records may be included with each IAT entry.

Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Addenda Record | M |
| *2* | 02-03 | 2 | '18' | Addenda Type Code | Addenda Record for Foreign Correspondent Bank Information for IAT | M |
| *3* | 04-38 | 35 | Alphanumeric | Foreign Correspondent Bank Name | This field contains the name of the Foreign Correspondent Bank | M |
| *4* | 39-40 | 2 | Alphanumeric | Foreign Correspondent Bank Identification Number Qualifier | This field contains a 2-digit code that identifies the numbering scheme used in the Foreign Correspondent Bank Identification Number field. Code values for this field are:<br>“01” = National Clearing System<br>“02” = BIC Code <br>“03” =IBAN Code | M |
| *5* | 41-74 | 34 | Alphanumeric | Foreign Correspondent Bank Identification Number | This field contains the bank ID number of the Foreign Correspondent Bank | M |
| *6* | 75-77 | 3 | Alphanumeric | Foreign Correspondent Bank Branch Country Code | This field contains the two-character code, as approved by the International Organization for Standardization (ISO), to identify the country in which the branch of the Foreign Correspondent Bank is located. Values can be found on the International Organization for Standardization website: www.iso.org | M |
| *7* | 78-83 | 6 | blank | Reserved | Leave blank | n/a |
| *8* | 84-87 | 4 | Numeric | Addenda Sequence Number | Sequence number of each „Type 18” Foreign Correspondent Bank Identification addenda in ascending order beginning with “0001” | M |
| *9* | 88-94 | 7 | Numeric | Entry Detail Sequence Number | This field contains the ascending sequence number section of the Entry Detail Record‟s trace number. This number is the same as the last seven digits of the trace number (Field 13) of the related Entry Detail Record. | M |

**IAT Addenda Record for IAT Returns (799)**

This Addenda Record is used to provide return data for a returned IAT entry

Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Addenda Record | M |
| *2* | 02-03 | 2 | '99' | Addenda Type Code | Addenda Record IAT Returns | M |
| *3* | 04-06 | 3 | Alphanumeric | Return Reason Code | This field contains code associated with the reason for the return | M |
| *4* | 07-21 | 15 | Numeric | Original Trace Number | This field contains the Trace Number as originally included on the forward Entry | M |
| *5* | 22-27 | 6 | YYMMDD | Date of Death | This field is used for Return Reason Code R14 or R15 | O |
| *6* | 28-35 | 8 | Alphanumeric | Original DFI | This field contains the Receiving DFI Identification as originally included on the forward Entry that the RDFI is returning or correcting | R |
| *7* | 36-45 | 10 | Numeric | Original Payment Amount | Original Payment of the forward Entry | R |
| *8* | 46-79 | 34 | Alphanumeric| Addenda Information | Addenda Information | O |
| *9* | 80-94 | 15 | Numeric | Trace Number | The Trace Number of the entry being returned | M |


## CTX Corporate Trade Exchange

The Corporate Trade Exchange (CTX) application provides the ability to collect and disburse funds and information between companies. Generally it is used by businesses paying one another for goods or services. These payments replace checks with an electronic process of debiting and crediting invoices between the financial institutions of participating companies.

**CTX Entry Detail Record**

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '6' | Record Type Code | Code Identifying the Entry Detail Record is '6' | M |
| *2* | 02-03 | 2 | Numeric | Transaction Code | Two-digit code that identifies checking account credits/debits. Valid codes are: <br>22 = Automated deposit (checking credit)<br>23 = Prenote of checking credit<br>24 = Zero-dollar checking credit with remittance data (CCD & CTX entries only)<br>27 = Automated payment (checking debit)<br>28 = Prenote of checking debit<br>29 = Zero-dollar checking debit with remittance data (CCD & CTX entries only)<br>32 = Automated deposit (savings credit)<br>33 = Prenote of savings credit<br>34 = Zero-dollar savings credit with remittance data (CCD & CTX entries only)<br>37 = Automated payment (savings debit)<br>38 = Prenote of savings debit<br>39 = Zero-dollar savings debit with remittance data (CCD & CTX entries only) | M |
| *3* | 04-11 | 8 | TTTTAAAA | Receiving DFI Identification | Routing Transit number of the receivers financial institution | M |
| *4* | 12-12 | 1 | Numeric | Check Digit | The ninth character of the RDFI Routing Transit Number. Used to check for transpositions. | M |
| *5* | 13-29 | 17 | Alphameric | DFI Account Number | Receiver's account number at the RDFI, a value found on the MICR line of a check| R |
| *6* | 30-39 | 10 | $$$$$$$$¢¢ | Amount | Entry amount in dollars with two decimal places. Right-justified, left zero-filled, without a decimal point. Enter 10 zeros for non-dollar prenote entries. | M |
| *7* | 40-54 | 15 | Alphameric | Identification Number | This field contains the accounting number by which the Receiver is known to the Originator for descriptive purposes. NACHA Rules recommend but do not require the RDFI to print the contents of this field on the receiver‟s statement. | R |
| *8* | 55-58 | 4 | Numeric | Number of Addenda Records | Number of addenda records associated with this transaction. | M |
| *9* | 59-74 | 16 | Alphameric | Receiving Company Name/ID Number | Name of Receiver. | M |
| *10* | 75-76 | 2 | blank | reserved | Leave blank | n/a |
| *11* | 77-78 | 2 | blank | Discretionary Data Field | The use of this field is defined by the ODFI.  | O |
| *12* | 79-79 | 1 | Numeric | Addenda Record Indicator | '1' = addenda included | O |
| *13* | 80-94 | 15 | Numeric | Trace Number | Standard Entry Detail Trace Number | M |

**CTX Addenda Record**

Up to 9,999 Addenda Records may be included with a CTX Entry Detail Record. The addenda should not be used with prenotes.

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '7' | Record Type Code | Code Identifying the Entry Detail Record is '7' | M |
| *2* | 02-03 | 2 | '05' | Addenda Type Code | The Addenda Type Code defines the specific interpretation and format for the addenda information contained in the Entry. | M |
| *3* | 04-83 | 80 | Alphameric | Payment Related Information | This field contains payment related ANSI ASC X12 data segments to further identify the payment or Transmit additional remittance information. | O |
| *4* | 84-87 | 4 | Numeric | Addenda Sequence Number | This number is consecutively assigned to each Addenda Record following an Entry Detail Record. The first addenda sequence number must always be a “1.” | M |
| *5* | 88-94 | 7 | Numeric | Entry Detail Sequence Number | This field contains the ascending sequence number section of the Entry Detail or Corporate Entry Detail Record’s trace number. This number is the same as the last seven digits of the trace number of the related Entry Detail Record or Corporate Entry Detail Record. | M |

## File Control Record - All Formats

The File Control record contains dollar, entry, and hash totals from the file's Company/Batch Control Records. This record also contains counts of the blocks and batches in the file.

| Field | Position | Size | Contents | Field Name | Entry Information | M,R,O |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| *1* | 01-01 | 1 | '9' | Record Type Code | Code Identifying the File Control Record is '9' | M |
| *2* | 02-07 |  | Numeric | Batch Count | Total number of Company/Batch Header Records (Record Type “5”) in the file. | M |
| *3* | 08-13 | 6 | Numeric | Block Count | Total number of physical blocks in the file, including the File Header and File Control Records. | M |
| *4* | 14-21 | 8 | Numeric | Entry / Addenda Count | Total number of Entry Detail and Addenda Records (Record Types “6” and “7”) in the file. | M |
| *5* | 22-31 | 10 | Numeric | Entry Hash | Total of eight character Transit Routing/ABA numbers in the file (Field 3 of the Entry Detail Record). Do not include the Transit Routing Check Digit. Enter the 10 low-order (right most) digits of this number. For example,if this sum is 998877665544, enter 8877665544. | M |
| *6* | 32-43 | 12 | $$$$$$$$$$¢¢ | Total Debit Entry Dollar Amount in File | Dollar total of debit entries in the file. If none, zero-fill the field. Do not enter a decimal point. Right-justify, left zero-fill. | M |
| *7* | 44-55 | 12 | $$$$$$$$$$¢¢ | Total Credit Entry Dollar Amount in File | Dollar total of credit entries in the file. If none, zero-fill the field. Do not enter a decimal point. Right-justify, left zero-fill. | M |
| *8* | 56-94 | 39 | blank | Reserved | Leave this field blank. | n/a |
