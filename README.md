# ObjCUnitConverter
Class Assignment, no functions or classes allowed.
//
//  main.m
//  LP1_measureConverter_dpope3
//
//  Created by Pope, Drewry on 3/27/17.
//  Copyright © 2017 assignment1 Drew Pope. All rights reserved.
//
				#import <Foundation/Foundation.h>

				int main(int argc, const char * argv[]) {
						@autoreleasepool {
								int numberOfTries = 0;
								char answer[4];
								char welcomeMessage[12] = "\n\nWelcome!";
								int conversionType;
								bool fromTypeNotValid;
								int fromType;
								double unitAmountInput;
								bool repeat = YES;

								const NSArray *_conversionTypes = @[ @[ @"inches", @"feet", @"yards", @"miles", @"millimeters", @"centimers", @"meters", @"kilometers"], @[ @"teaspoons", @"tablespoons", @"cups"]]; //,@"pint", @"quart", @"gallons"]];

								const NSDictionary *unitFactor = @{ @"inches" :         @[[NSNumber numberWithDouble: 25.4], @4],
																						@"feet" :           @[[NSNumber numberWithDouble:304.8], @3],
																						@"yards" :          @[[NSNumber numberWithDouble:914.3], @3],
																						@"miles" :          @[[NSNumber numberWithDouble:1609344], @4],
																						@"millimeters" :    @[[NSNumber numberWithDouble:1], @2],
																						@"centimers" :      @[[NSNumber numberWithDouble:10], @2],
																						@"meters" :         @[[NSNumber numberWithDouble:1000], @3],
																						@"kilometers" :     @[[NSNumber numberWithDouble:1000000],@4],
																						@"teaspoons" :      @[[NSNumber numberWithDouble:1], @2],
																						@"tablespoons" :    @[[NSNumber numberWithDouble:3], @2],
																						@"cups" :           @[[NSNumber numberWithDouble:64], @3]};

				do {
						numberOfTries += 1;

						//I put the welcome message within this NSLog, instead of as it's own NSLog to cut down on system prompt displays.
						NSLog(@"%s\n\nWhich type of conversion do you wish to do?\n\n1\t Length/Distance\n2\t Cooking\n\n\n#%i: Enter # for conversion type you wish to do: ", welcomeMessage, numberOfTries);

						//https://stackoverflow.com/questions/1673352/how-to-check-a-character-array-is-null-in-objective-c
						if (numberOfTries == 1) memset(welcomeMessage, 0, sizeof(welcomeMessage));

						scanf("%d", &conversionType);

						if(conversionType == 1 || conversionType == 2) {

								//Reset try count, align entered integer with array index. Set validation variable.
								numberOfTries = 0;
								conversionType -= 1;
								fromTypeNotValid = YES;
								NSMutableString *inputTypeMessage = [[NSMutableString alloc] initWithFormat: @"\n\nWhat type of %@ data would you like to convert from?\n\n", (conversionType < 1 ? @"length" : @"cooking")];

								for (int dataTypeIndex = 0; dataTypeIndex < [_conversionTypes[conversionType] count]; dataTypeIndex += 1) {
										[inputTypeMessage appendFormat: @"%i\t%@\n", dataTypeIndex + 1, _conversionTypes[conversionType][dataTypeIndex]];
								}

								do {
										numberOfTries += 1;
										NSLog(@"%@\n#%i: Enter # for type you wish to convert from:\n", inputTypeMessage, numberOfTries);
										scanf("%d", &fromType);
										if (fromType > 0 && fromType <= [_conversionTypes[conversionType] count]) fromTypeNotValid = NO;
								} while(fromTypeNotValid);

								fromType -= 1;
								NSLog(@"\n\nEnter the number of %@ you wish to convert:", _conversionTypes[conversionType][fromType]);
								scanf("%lf", &unitAmountInput);
								NSMutableString *outputMessage = [[NSMutableString alloc] initWithFormat: @"\n\n%f %@ is the equivalent of: \n\n", unitAmountInput, _conversionTypes[conversionType][fromType] ];

								for(int dataTypeIndex = 0; dataTypeIndex < [_conversionTypes[conversionType] count] ; dataTypeIndex += 1) {
										//if not input data type.
										if (dataTypeIndex != fromType) {
												//Pull input format from dictionary, convert down to base unit for input conversion type, then multiply up to current conversion type's factor of base unit. (mm for length), output this number and also data type
												[outputMessage appendFormat: @"%@ %@", [NSNumber numberWithDouble:((unitAmountInput * [[unitFactor objectForKey: _conversionTypes[conversionType][fromType]][0] doubleValue]) / [[unitFactor objectForKey: _conversionTypes[conversionType][dataTypeIndex]][0] doubleValue])], _conversionTypes[conversionType][dataTypeIndex]];

												//if: NOT multiple of 4, NOT second to last data conversion type AND chosen input type is last data conversion type, NOT last data conversion type -> THEN ADD "   OR    "
												if (!((dataTypeIndex + 1) % 4 == 0) && !(fromType == (dataTypeIndex +1) && ((dataTypeIndex + 1) != [_conversionTypes[conversionType] count])) && ((dataTypeIndex + 1) != [_conversionTypes[conversionType] count])) [outputMessage appendFormat: @"     OR     "];
										}
										if ((dataTypeIndex + 1) % 4 == 0) [outputMessage appendFormat: @"\n\n"];
								}
								NSLog(@"%@\n\nDo you wish to continue doing more conversions (y/n)?", outputMessage);
								scanf("\n%[^\n]", answer);
								repeat = answer[0] == 'y' ? YES:NO;
						}
				} while (repeat == YES);
				if (answer[0] != 'n') NSLog(@"\nProgram ended with exit code: 0");
		}
		return 0;
}
